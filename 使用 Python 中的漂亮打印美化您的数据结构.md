处理数据对于任何 Pythonista 都是必不可少的，但有时这些数据并不是很漂亮。计算机不关心格式，但如果没有好的格式，人类可能会发现一些难以阅读的东西。当您print()在大型字典或长列表上使用时，输出并不漂亮——它很有效，但并不漂亮。

将pprint在Python模块是一种实用工具模块，您可以使用打印数据结构以可读的，漂亮的方式。它是标准库的一部分，对于调试处理 API 请求、大型 JSON 文件和一般数据的代码特别有用。

在本教程结束时，您将：

明白为什么该pprint模块是必要的
了解如何使用pprint()，PrettyPrinter以及它们的参数
能够创建自己的实例 PrettyPrinter
保存格式化的字符串输出而不是打印它
打印和识别递归数据结构
在此过程中，您还将看到对公共 API 和JSON 解析的 HTTP 请求。

理解对 Python 漂亮打印的需求
Pythonpprint模块在许多情况下都很有用。在发出 API 请求、处理JSON 文件或处理复杂和嵌套的数据时，它会派上用场。您可能会发现使用普通 print()函数不足以有效地探索数据和调试 应用程序。当您print()与字典 和列表一起使用时，输出不包含任何换行符。

在开始探索之前pprint，您将首先使用urllib发出请求以获取一些数据。您将向 {JSON} Placeholder 请求一些模拟用户信息。首先要做的是发出 HTTPGET请求并将响应放入字典中：

>>>
>>> from urllib import request
>>> response = request.urlopen("https://jsonplaceholder.typicode.com/users")
>>> json_response = response.read()
>>> import json
>>> users = json.loads(json_response)
在这里，您发出一个基本GET请求，然后将响应解析为带有json.loads(). 字典现在在一个变量中，一个常见的下一步是打印内容print()：

>>>
>>> print(users)
[{'id': 1, 'name': 'Leanne Graham', 'username': 'Bret', 'email': 'Sincere@april.biz', 'address': {'street': 'Kulas Light', 'suite': 'Apt. 556', 'city': 'Gwenborough', 'zipcode': '92998-3874', 'geo': {'lat': '-37.3159', 'lng': '81.1496'}}, 'phone': '1-770-736-8031 x56442', 'website': 'hildegard.org', 'company': {'name': 'Romaguera-Crona', 'catchPhrase': 'Multi-layered client-server neural-net', 'bs': 'harness real-time e-markets'}}, {'id': 2, 'name': 'Ervin Howell', 'username': 'Antonette', 'email': 'Shanna@melissa.tv', 'address': {'street': 'Victor Plains', 'suite': 'Suite 879', 'city': 'Wisokyburgh', 'zipcode': '90566-7771', 'geo': {'lat': '-43.9509', 'lng': '-34.4618'}}, 'phone': '010-692-6593 x09125', 'website': 'anastasia.net', 'company': {'name': 'Deckow-Crist', 'catchPhrase': 'Proactive didactic contingency', 'bs': 'synergize scalable supply-chains'}}, {'id': 3, 'name': 'Clementine Bauch', 'username': 'Samantha', 'email': 'Nathan@yesenia.net', 'address': {'street': 'Douglas Extension', 'suite': 'Suite 847', 'city': 'McKenziehaven', 'zipcode': '59590-4157', 'geo': {'lat': '-68.6102', 'lng': '-47.0653'}}, 'phone': '1-463-123-4447', 'website': 'ramiro.info', 'company': {'name': 'Romaguera-Jacobson', 'catchPhrase': 'Face to face bifurcated interface', 'bs': 'e-enable strategic applications'}}, {'id': 4, 'name': 'Patricia Lebsack', 'username': 'Karianne', 'email': 'Julianne.OConner@kory.org', 'address': {'street': 'Hoeger Mall', 'suite': 'Apt. 692', 'city': 'South Elvis', 'zipcode': '53919-4257', 'geo': {'lat': '29.4572', 'lng': '-164.2990'}}, 'phone': '493-170-9623 x156', 'website': 'kale.biz', 'company': {'name': 'Robel-Corkery', 'catchPhrase': 'Multi-tiered zero tolerance productivity', 'bs': 'transition cutting-edge web services'}}, {'id': 5, 'name': 'Chelsey Dietrich', 'username': 'Kamren', 'email': 'Lucio_Hettinger@annie.ca', 'address': {'street': 'Skiles Walks', 'suite': 'Suite 351', 'city': 'Roscoeview', 'zipcode': '33263', 'geo': {'lat': '-31.8129', 'lng': '62.5342'}}, 'phone': '(254)954-1289', 'website': 'demarco.info', 'company': {'name': 'Keebler LLC', 'catchPhrase': 'User-centric fault-tolerant solution', 'bs': 'revolutionize end-to-end systems'}}, {'id': 6, 'name': 'Mrs. Dennis Schulist', 'username': 'Leopoldo_Corkery', 'email': 'Karley_Dach@jasper.info', 'address': {'street': 'Norberto Crossing', 'suite': 'Apt. 950', 'city': 'South Christy', 'zipcode': '23505-1337', 'geo': {'lat': '-71.4197', 'lng': '71.7478'}}, 'phone': '1-477-935-8478 x6430', 'website': 'ola.org', 'company': {'name': 'Considine-Lockman', 'catchPhrase': 'Synchronised bottom-line interface', 'bs': 'e-enable innovative applications'}}, {'id': 7, 'name': 'Kurtis Weissnat', 'username': 'Elwyn.Skiles', 'email': 'Telly.Hoeger@billy.biz', 'address': {'street': 'Rex Trail', 'suite': 'Suite 280', 'city': 'Howemouth', 'zipcode': '58804-1099', 'geo': {'lat': '24.8918', 'lng': '21.8984'}}, 'phone': '210.067.6132', 'website': 'elvis.io', 'company': {'name': 'Johns Group', 'catchPhrase': 'Configurable multimedia task-force', 'bs': 'generate enterprise e-tailers'}}, {'id': 8, 'name': 'Nicholas Runolfsdottir V', 'username': 'Maxime_Nienow', 'email': 'Sherwood@rosamond.me', 'address': {'street': 'Ellsworth Summit', 'suite': 'Suite 729', 'city': 'Aliyaview', 'zipcode': '45169', 'geo': {'lat': '-14.3990', 'lng': '-120.7677'}}, 'phone': '586.493.6943 x140', 'website': 'jacynthe.com', 'company': {'name': 'Abernathy Group', 'catchPhrase': 'Implemented secondary concept', 'bs': 'e-enable extensible e-tailers'}}, {'id': 9, 'name': 'Glenna Reichert', 'username': 'Delphine', 'email': 'Chaim_McDermott@dana.io', 'address': {'street': 'Dayna Park', 'suite': 'Suite 449', 'city': 'Bartholomebury', 'zipcode': '76495-3109', 'geo': {'lat': '24.6463', 'lng': '-168.8889'}}, 'phone': '(775)976-6794 x41206', 'website': 'conrad.com', 'company': {'name': 'Yost and Sons', 'catchPhrase': 'Switchable contextually-based project', 'bs': 'aggregate real-time technologies'}}, {'id': 10, 'name': 'Clementina DuBuque', 'username': 'Moriah.Stanton', 'email': 'Rey.Padberg@karina.biz', 'address': {'street': 'Kattie Turnpike', 'suite': 'Suite 198', 'city': 'Lebsackbury', 'zipcode': '31428-2261', 'geo': {'lat': '-38.2386', 'lng': '57.2232'}}, 'phone': '024-648-3804', 'website': 'ambrose.net', 'company': {'name': 'Hoeger LLC', 'catchPhrase': 'Centralized empowering task-force', 'bs': 'target end-to-end models'}}]
哦亲爱的！没有换行符的一大行。根据您的控制台设置，这可能显示为一个很长的行。或者，您的控制台输出可能启用了自动换行模式，这是最常见的情况。不幸的是，这并没有使输出更友好！

如果您查看第一个和最后一个字符，您会发现这似乎是一个列表。您可能想开始编写一个循环来打印项目：

for user in users:
    print(user)
这个for循环会将每个对象打印在单独的一行上，但即便如此，每个对象占用的空间也比一行多。以这种方式打印确实会使事情变得更好，但绝不是理想的。上面的例子是一个比较简单的数据结构，但是你会用 100 倍大小的深度嵌套字典做什么？

当然，您可以编写一个函数，使用递归 找到打印所有内容的方法。不幸的是，您可能会遇到一些无法正常工作的边缘情况。您甚至可能会发现自己编写了一个完整的函数模块，只是为了掌握数据的结构！

进入pprint模块！

Working Withpprint
pprint是一个 Python 模块，用于以漂亮的方式打印数据结构。它长期以来一直是 Python 标准库的一部分，因此没有必要单独安装它。你需要做的就是导入它的pprint()函数：

>>>
>>> from pprint import pprint
然后，print(users)您可以调用新的最喜欢的函数来使输出变得漂亮，而不是像上面的示例那样使用常规方法：

>>>
>>> pprint(users)
这个函数打印users——但以一种新的和改进的漂亮方式：

>>>
>>> pprint(users)
[{'address': {'city': 'Gwenborough',
              'geo': {'lat': '-37.3159', 'lng': '81.1496'},
              'street': 'Kulas Light',
              'suite': 'Apt. 556',
              'zipcode': '92998-3874'},
  'company': {'bs': 'harness real-time e-markets',
              'catchPhrase': 'Multi-layered client-server neural-net',
              'name': 'Romaguera-Crona'},
  'email': 'Sincere@april.biz',
  'id': 1,
  'name': 'Leanne Graham',
  'phone': '1-770-736-8031 x56442',
  'username': 'Bret',
  'website': 'hildegard.org'},
 {'address': {'city': 'Wisokyburgh',
              'geo': {'lat': '-43.9509', 'lng': '-34.4618'},
              'street': 'Victor Plains',
              'suite': 'Suite 879',
              'zipcode': '90566-7771'},
  'company': {'bs': 'synergize scalable supply-chains',
              'catchPhrase': 'Proactive didactic contingency',
              'name': 'Deckow-Crist'},
  'email': 'Shanna@melissa.tv',
  'id': 2,
  'name': 'Ervin Howell',
  'phone': '010-692-6593 x09125',
  'username': 'Antonette',
  'website': 'anastasia.net'},

 ...

 {'address': {'city': 'Lebsackbury',
              'geo': {'lat': '-38.2386', 'lng': '57.2232'},
              'street': 'Kattie Turnpike',
              'suite': 'Suite 198',
              'zipcode': '31428-2261'},
  'company': {'bs': 'target end-to-end models',
              'catchPhrase': 'Centralized empowering task-force',
              'name': 'Hoeger LLC'},
  'email': 'Rey.Padberg@karina.biz',
  'id': 10,
  'name': 'Clementina DuBuque',
  'phone': '024-648-3804',
  'username': 'Moriah.Stanton',
  'website': 'ambrose.net'}]
多么漂亮！字典的键甚至在视觉上缩进了！此输出使扫描和可视化分析数据结构变得更加简单。

注意：如果您自己运行代码，您将看到的输出会更长。此代码块为了可读性而截断了输出。

如果您喜欢尽可能少打字，那么您会很高兴知道它pprint()有一个别名pp()：

>>>
>>> from pprint import pp
>>> pp(users)
pp()只是一个包装器pprint()，它的行为方式完全相同。

注意： Python 从3.8.0 alpha 2 版本开始包含这个别名。

但是，即使是默认输出，一开始也可能需要扫描太多信息。也许您真正想要的只是验证您是否正在处理一个普通对象列表。为此，您需要稍微调整输出。

对于这些情况，您可以传递各种参数pprint()来使最简洁的数据结构变得漂亮。

探索可选参数 pprint()
在本节中，您将了解可用于pprint(). 您可以使用七个参数来配置 Pythonic 漂亮的打印机。你不需要全部使用它们，有些会比其他的更有用。你会发现最有价值的可能是depth。

总结您的数据： depth
最容易使用的参数之一是depth. users 如果数据结构处于或低于指定的深度，以下 Python 命令将仅打印 的全部内容——当然，同时保持内容美观。更深的数据结构的内容用三个点代替：

>>>
>>> pprint(users, depth=1)
[{...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}]
现在您可以立即看到这确实是一个字典列表。要进一步探索数据结构，您可以将深度增加一级，这将打印字典的所有顶级键users：

>>>
>>> pprint(users, depth=2)
[{'address': {...},
  'company': {...},
  'email': 'Sincere@april.biz',
  'id': 1,
  'name': 'Leanne Graham',
  'phone': '1-770-736-8031 x56442',
  'username': 'Bret',
  'website': 'hildegard.org'},
 {'address': {...},
  'company': {...},
  'email': 'Shanna@melissa.tv',
  'id': 2,
  'name': 'Ervin Howell',
  'phone': '010-692-6593 x09125',
  'username': 'Antonette',
  'website': 'anastasia.net'},

  ...

 {'address': {...},
  'company': {...},
  'email': 'Rey.Padberg@karina.biz',
  'id': 10,
  'name': 'Clementina DuBuque',
  'phone': '024-648-3804',
  'username': 'Moriah.Stanton',
  'website': 'ambrose.net'}]
现在您可以快速检查所有词典是否共享其顶级键。这是一个很有价值的观察，尤其是当您的任务是开发一个使用此类数据的应用程序时。

给你的数据空间： indent
该indent参数控制漂亮打印表示的每个级别在输出中的缩进程度。默认缩进是 just 1，它转换为一个空格字符：

>>>
>>> pprint(users[0], depth=1)
{'address': {...},
 'company': {...},
 'email': 'Sincere@april.biz',
 'id': 1,
 'name': 'Leanne Graham',
 'phone': '1-770-736-8031 x56442',
 'username': 'Bret',
 'website': 'hildegard.org'}

>>> pprint(users[0], depth=1, indent=4)
{   'address': {...},
    'company': {...},
    'email': 'Sincere@april.biz',
    'id': 1,
    'name': 'Leanne Graham',
    'phone': '1-770-736-8031 x56442',
    'username': 'Bret',
    'website': 'hildegard.org'}
缩进行为的最重要部分pprint() 是保持所有键在视觉上对齐。应用多少缩进取决于indent参数和键的位置。

由于上面的示例中没有嵌套，因此缩进量完全基于indent参数。在这两个示例中，请注意左大括号 ( {) 是如何计为第一个键的缩进单位的。在第一个示例中，第一个键的开头单引号紧随其后{，中间没有任何空格，因为缩进设置为1。

但是，当存在嵌套时，缩进应用于第一个行内元素，pprint()然后保持所有后续元素与第一个元素对齐。因此，如果您在打印时设置indent为，第一个元素将缩进四个字符，而嵌套元素将缩进八个以上字符，因为缩进从第一个键的末尾开始：4users

>>>
>>> pprint(users[0], depth=2, indent=4)
{   'address': {   'city': 'Gwenborough',
                   'geo': {...},
                   'street': 'Kulas Light',
                   'suite': 'Apt. 556',
                   'zipcode': '92998-3874'},
    'company': {   'bs': 'harness real-time e-markets',
                   'catchPhrase': 'Multi-layered client-server neural-net',
                   'name': 'Romaguera-Crona'},
    'email': 'Sincere@april.biz',
    'id': 1,
    'name': 'Leanne Graham',
    'phone': '1-770-736-8031 x56442',
    'username': 'Bret',
    'website': 'hildegard.org'}
这只是Python 中漂亮的另一部分pprint()！

限制你的线长： width
默认情况下，pprint()每行最多只能输出八十个字符。您可以通过传入width参数来自定义此值。 pprint()将努力将内容放在一行中。如果数据结构的内容超过此限制，则它将在新行上打印当前数据结构的每个元素：

>>>
>>> pprint(users[0])
{'address': {'city': 'Gwenborough',
             'geo': {'lat': '-37.3159', 'lng': '81.1496'},
             'street': 'Kulas Light',
             'suite': 'Apt. 556',
             'zipcode': '92998-3874'},
 'company': {'bs': 'harness real-time e-markets',
             'catchPhrase': 'Multi-layered client-server neural-net',
             'name': 'Romaguera-Crona'},
 'email': 'Sincere@april.biz',
 'id': 1,
 'name': 'Leanne Graham',
 'phone': '1-770-736-8031 x56442',
 'username': 'Bret',
 'website': 'hildegard.org'}
当您将宽度保留为默认值 80 个字符时，字典 atusers[0]['address']['geo'] 仅包含 a'lat'和 a'lng'属性。这意味着将缩进和打印字典所需的字符数（包括其间的空格）相加，不到八十个字符。由于它少于八十个字符，因此默认宽度 pprint()将其全部放在一行中。

但是，字典 atusers[0]['company']会超过默认宽度，因此pprint()将每个键放在一个新行上。字典、列表、元组和集合也是如此：

>>>
>>> pprint(users[0], width=160)
{'address': {'city': 'Gwenborough', 'geo': {'lat': '-37.3159', 'lng': '81.1496'}, 'street': 'Kulas Light', 'suite': 'Apt. 556', 'zipcode': '92998-3874'},
 'company': {'bs': 'harness real-time e-markets', 'catchPhrase': 'Multi-layered client-server neural-net', 'name': 'Romaguera-Crona'},
 'email': 'Sincere@april.biz',
 'id': 1,
 'name': 'Leanne Graham',
 'phone': '1-770-736-8031 x56442',
 'username': 'Bret',
 'website': 'hildegard.org'}
如果您将宽度设置为一个较大的值，例如160，则所有嵌套字典都适合在一行中。您甚至可以将其发挥到极致，并使用一个巨大的值500，例如 ，在此示例中，将整个字典打印在一行上：

>>>
>>> pprint(users[0], width=500)
{'address': {'city': 'Gwenborough', 'geo': {'lat': '-37.3159', 'lng': '81.1496'}, 'street': 'Kulas Light', 'suite': 'Apt. 556', 'zipcode': '92998-3874'}, 'company': {'bs': 'harness real-time e-markets', 'catchPhrase': 'Multi-layered client-server neural-net', 'name': 'Romaguera-Crona'}, 'email': 'Sincere@april.biz', 'id': 1, 'name': 'Leanne Graham', 'phone': '1-770-736-8031 x56442', 'username': 'Bret', 'website': 'hildegard.org'}
在这里，您可以获得设置width 为相对较大值的效果。您可以采用另一种方式并设置width为较低的值，例如1。然而，这将产生的主要影响是确保每个数据结构将在单独的行上显示其组件。您仍将获得排列组件的视觉缩进：

>>>
>>> pprint(users[0], width=5)
{'address': {'city': 'Gwenborough',
             'geo': {'lat': '-37.3159',
                     'lng': '81.1496'},
             'street': 'Kulas '
                       'Light',
             'suite': 'Apt. '
                      '556',
             'zipcode': '92998-3874'},
 'company': {'bs': 'harness '
                   'real-time '
                   'e-markets',
             'catchPhrase': 'Multi-layered '
                            'client-server '
                            'neural-net',
             'name': 'Romaguera-Crona'},
 'email': 'Sincere@april.biz',
 'id': 1,
 'name': 'Leanne '
         'Graham',
 'phone': '1-770-736-8031 '
          'x56442',
 'username': 'Bret',
 'website': 'hildegard.org'}
很难让 Pythonpprint()打印出丑陋的东西。它会竭尽全力变得漂亮！

在此示例中，除了了解 之外width，您还将探索打印机如何拆分长文本行。请注意users[0]["company"]["catchPhrase"]最初是 的 是 如何'Multi-layered client-server neural-net'在每个空间上拆分的。打印机避免在单词中间分割此字符串，因为这会使其难以阅读。

压缩你的长序列： compact
您可能认为这compact指的是您在有关的部分中探索的行为width——也就是说，是compact让数据结构出现在一行还是单独的行。然而，compact只有一次线会影响输出上的width。

注意： compact只影响序列的输出：列表、集合和元组，而不影响字典。这是有意为之，但尚不清楚为什么会做出此决定。在Python 问题 #34798 中有一个正在进行的讨论 。

如果compact是True，则输出将换行到下一行。如果数据结构长于宽度，则默认行为是每个元素都出现在自己的行上：

>>>
>>> pprint(users, depth=1)
[{...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}, {...}]

>>> pprint(users, depth=1, width=40)
[{...},
 {...},
 {...},
 {...},
 {...},
 {...},
 {...},
 {...},
 {...},
 {...}]

>>> pprint(users, depth=1, width=40, compact=True)
[{...}, {...}, {...}, {...}, {...},
 {...}, {...}, {...}, {...}, {...}]
使用默认设置漂亮地打印此列表会在一行中打印出缩写版本。限制width到40人物，你强制pprint()输出所有列表的在不同的行元素。如果然后设置compact=True，则列表将包含 40 个字符，并且比通常看起来更紧凑。

注意：请注意，将宽度设置为小于 7 个字符——在这种情况下，相当于[{...},输出——似乎depth完全绕过了参数，pprint()最终打印所有内容而没有任何折叠。这已被报告为错误 #45611。

compact 对于具有短元素的长序列很有用，否则这些元素会占用多行并降低输出的可读性。

指导你的输出： stream
该stream参数指的是 的输出pprint()。默认情况下，它会去往同一个地方print()。具体来说，它转到 sys.stdout，它实际上是 Python 中的 文件对象。但是，您可以将其重定向到任何文件对象，就像您可以使用print()：

>>>
>>> with open("output.txt", mode="w") as file_object:
...     pprint(users, stream=file_object)
在这里，您使用 来创建文件对象 open()，然后将stream参数设置pprint()为该文件对象。如果您随后打开该output.txt文件，您应该会看到users里面的所有内容都打印得很漂亮。

Python 确实有自己的 日志记录模块。但是，您也可以使用pprint()将漂亮的输出发送到文件，并根据需要将它们用作日志。

防止字典排序： sort_dicts
尽管字典通常被认为是无序的数据结构，但从 Python 3.6 开始， 字典按插入排序。

pprint() 按字母顺序排列键以进行打印：

>>>
>>> pprint(users[0], depth=1)
{'address': {...},
 'company': {...},
 'email': 'Sincere@april.biz',
 'id': 1,
 'name': 'Leanne Graham',
 'phone': '1-770-736-8031 x56442',
 'username': 'Bret',
 'website': 'hildegard.org'}

>>> pprint(users[0], depth=1, sort_dicts=False)
{'id': 1,
 'name': 'Leanne Graham',
 'username': 'Bret',
 'email': 'Sincere@april.biz',
 'address': {...},
 'phone': '1-770-736-8031 x56442',
 'website': 'hildegard.org',
 'company': {...}}
除非您设置sort_dicts为False，否则Python 会pprint()按字母顺序对键进行排序。它使字典的输出保持一致、可读，而且——嗯——漂亮！

当pprint()第一次实施，字典是无序的。如果没有按字母顺序排列键，字典的键理论上可能在每次打印时都不同。

美化你的数字： underscore_numbers
该underscore_numbers参数是Python 3.10中引入的一个特性，它使长数字更具可读性。考虑到您目前使用的示例不包含任何长数字，您需要一个新示例来尝试一下：

>>>
>>> number_list = [123456789, 10000000000000]
>>> pprint(number_list, underscore_numbers=True)
[123_456_789, 10_000_000_000_000]
如果您尝试运行此调用pprint()并出现错误，那么您并不孤单。截至 2021 年 10 月，此参数在pprint()直接调用时不起作用。Python 社区很快就注意到了这一点，并已 在 2021 年 12 月的3.10.1 错误修复版本中修复。Python 的人关心他们漂亮的打印机！当您阅读本教程时，他们可能已经解决了这个问题。

如果underscore_numbers在您pprint()直接调用时不起作用并且您确实想要漂亮的数字，则有一个解决方法：当您创建自己的PrettyPrinter对象时，此参数应该像在上面的示例中一样起作用。

接下来，您将介绍如何创建PrettyPrinter对象。

创建自定义PrettyPrinter对象
可以创建一个PrettyPrinter 具有您定义的默认值的实例。一旦有了自定义PrettyPrinter对象的新实例，就可以通过调用实例.pprint()上的方法来使用它PrettyPrinter：

>>>
>>> from pprint import PrettyPrinter
>>> custom_printer = PrettyPrinter(
...     indent=4,
...     width=100,
...     depth=2,
...     compact=True,
...     sort_dicts=False,
...     underscore_numbers=True
... )
...
>>> custom_printer.pprint(users[0])
{   'id': 1,
    'name': 'Leanne Graham',
    'username': 'Bret',
    'email': 'Sincere@april.biz',
    'address': {   'street': 'Kulas Light',
                   'suite': 'Apt. 556',
                   'city': 'Gwenborough',
                   'zipcode': '92998-3874',
                   'geo': {...}},
    'phone': '1-770-736-8031 x56442',
    'website': 'hildegard.org',
    'company': {   'name': 'Romaguera-Crona',
                   'catchPhrase': 'Multi-layered client-server neural-net',
                   'bs': 'harness real-time e-markets'}}
>>> number_list = [123456789, 10000000000000]
>>> custom_printer.pprint(number_list)
[123_456_789, 10_000_000_000_000]
使用这些命令，您可以：

Imported PrettyPrinter，这是一个类定义
使用某些参数创建了该类的新实例
打印第一个用户users
定义了几个长数字的列表
Printednumber_list，这也在underscore_numbers行动中演示
请注意，您传递给的参数PrettyPrinter 与默认pprint()参数完全相同，只是您跳过了第一个参数。在 中pprint()，这是您要打印的对象。

通过这种方式，您可以拥有各种打印机预设——也许有些会进入不同的流——并在需要时调用它们。

得到一个漂亮的字符串 pformat()
如果您不想将漂亮的输出发送pprint()到流怎么办？也许您想做一些 正则表达式 匹配并替换某些键。对于普通字典，您可能会发现自己想要删除方括号和引号，使它们看起来更易读。

无论您想对字符串预输出做什么，您都可以使用pformat()以下命令获取字符串 ：

>>>
>>> from pprint import pformat
>>> address = pformat(users[0]["address"])
>>> chars_to_remove = ["{", "}", "'"]
>>> for char in chars_to_remove:
...     address = address.replace(char, "")
...
>>> print(address)
city: Gwenborough,
 geo: lat: -37.3159, lng: 81.1496,
 street: Kulas Light,
 suite: Apt. 556,
 zipcode: 92998-3874
pformat() 是一种可用于在漂亮的打印机和输出流之间切换的工具。

另一个用例可能是，如果您正在 构建 API 并希望发送 JSON 字符串的漂亮字符串表示。您的最终用户可能会喜欢它！

处理递归数据结构
Pythonpprint()是递归的，这意味着它将漂亮地打印字典的所有内容、任何子字典的所有内容，等等。

问问自己当递归函数遇到递归数据结构时会发生什么。想象一下你有字典A和字典B：

A有一个属性 ，.link指向B。
B有一个属性 ，.link指向A。
如果你想象中的递归函数无法处理这个循环引用，它永远不会完成打印！它会打印A，然后是它的子项B. 但B也有A作为一个孩子，所以它会进入无穷大。

幸运的是，普通print()函数和pprint()函数都能很好地处理这个问题：

>>>
>>> A = {}
>>> B = {"link": A}
>>> A["link"] = B
>>> print(A)
{'link': {'link': {...}}}
>>> from pprint import pprint
>>> pprint(A)
{'link': {'link': <Recursion on dict with id=3032338942464>}}
虽然 Python 的常规print()只是缩写输出， pprint()明确通知您递归并添加字典的 ID。

如果你想探索为什么这个结构是递归的，你可以了解更多关于 通过引用传递的信息。

结论
您已针对的主要用途pprint在Python模块和一些方法来工作，pprint()和PrettyPrinter。pprint()当您开发处理复杂数据结构的东西时，您会发现这特别方便。也许您正在开发一个使用不熟悉的 API 的应用程序。也许您有一个充满深度嵌套 JSON 文件的数据仓库。这些都是pprint可以派上用场的情况。

在本教程中，您学习了如何：

导入 pprint以在您的程序中使用
使用pprint()替代常规的print()
了解可用于自定义漂亮打印输出的所有参数
在打印之前将格式化输出作为字符串获取
创建自定义实例 PrettyPrinter
认识递归数据结构以及如何pprint()处理它们
为了帮助您掌握函数和参数，您使用了一个代表某些用户的数据结构示例。您还探索了一些可能使用pprint().

恭喜！您现在可以通过使用 Python 的pprint模块更好地处理复杂数据。
