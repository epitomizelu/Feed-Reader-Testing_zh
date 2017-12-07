# 项目说明
1，该项目演示了如何使用jasmine做开发测试;
2,项目GITPAGE 托管地址：https://epitomizelu.github.io/Feed-Reader-Testing_zh/;
3,可以将本项目克隆到本地，部署在本地服务器，访问项目根目录下的index.html文件



# 项目总结（jasmine测试总结）
##  一，jasmine是js的测试框架
官网地址：[https://jasmine.github.io/pages/getting_started.html](https://jasmine.github.io/pages/getting_started.html)

>BDD和TDD
>>BDD，behaviour driven devolepment，行为驱动开发，从用户的需求角度出发，强调系统行为。比如测试左侧菜单的显示与隐藏，切换选项卡之后内容是否真的发生了改变等，关注系统的功能和行为。

>>TDD，test driven devolepment，测试驱动开发，以函数为单位的测试，即调用一个函数，看函数的返回结果是否符合预期。

>> 我理解 BDD必然包含TDD，TDD不一样要BDD。

##  二，如何在工程中加入jasmine
### 1，下载 jasmine压缩包
```html
下载地址
[https://github.com/jasmine/jasmine/releases](https://github.com/jasmine/jasmine/releases)
```

### 2，在你的工程根目录下新建一个文件夹，取名jasmine
```bash
mkdir my-project/jasmine
```

### 3，将下载的压缩包move到新建的jasmine目录下
```bash
mv jasmine/dist/jasmine-standalone-{#.#.#}.zip my-project/jasmine
```

### 4，在jasmine目录下解压缩
```bash
cd my-project/jasmine
unzip jasmine-standalone-{#.#.#}.zip
```

### 5，将jasmine必要的css和js文件引入到你的html文件中
```html
<link rel="shortcut icon" type="image/png" href="jasmine/lib/jasmine-{#.#.#}/jasmine_favicon.png">
<link rel="stylesheet" type="text/css" href="jasmine/lib/jasmine-{#.#.#}/jasmine.css">

<script type="text/javascript" src="jasmine/lib/jasmine-{#.#.#}/jasmine.js"></script>
<script type="text/javascript" src="jasmine/lib/jasmine-{#.#.#}/jasmine-html.js"></script>
<script type="text/javascript" src="jasmine/lib/jasmine-{#.#.#}/boot.js"></script>

```

###  6，在你的jasmine目录下新建一个文件夹取名spec，用来放置你的测试用例文件
###  7，在你的新建的文件夹spec下新建一个文件取名为ContactSpec.js，用来编写你针对某个功能编写的测试用例

> ps:到此你就可以使用jsamine框架写单元测试了

# 三，如何编写测试用例

> ps:以下的文件名、目录名、函数名都是随便取的，不属于jasmine框架规范


### 1，新建ContactSpec.js文件是以测试用例为单位组织的，测试用例在js语法上表现为调用一个以字符串和函数为参数的函数
1.1，参数一：字符串。用来描述测试用例，在测试反馈结果页面上体现为标题(如图中黑色的标题RSS Feeds、The menu、Initial Entries、New Feed Selection
![](http://ov4zbm8w2.bkt.clouddn.com/image/js/jasminjasmine-result.png)

1.2，参数二，函数。函数里面编写多个具体的测试

1.3，demo如下

```javascript

/*这是一个测试用例，以调用describe方法开始，两个参数，第一个字符串是关于这个测试用例的简要描述*/
describe('RSS Feeds', function() {

        /* 这是我们的第一个测试 */      
        it('are defined', function() {
            expect(allFeeds).toBeDefined();
            expect(allFeeds.length).not.toBe(0);
        });


        /* 
        * 编写一个测试遍历 allFeeds 对象里面的所有的源来保证有链接字段而且链接不是空的。
        */
        it('url of every feed should not null',function() {
            allFeeds.forEach(function(item) {
                expect(item.url).not.toBeNull();
            });
        })

        /* 
        * 编写一个测试遍历 allFeeds 对象里面的所有的源来保证有名字字段而且不是空的。
        */ 
        it('name of every feed should not null',function() {
            allFeeds.forEach(function(item) {
                expect(item.name).not.toBeNull();
            });
        })

    });

```
1.4 将ContactSpec.js导入到你的html文件中,然后把你的网站跑起来，在对应的html文件中可以看到响应的测试结果
```html
<script type="text/javascript" src="jasmine/spec/ContactSpec.js"></script>

```

### 2，如何编写单个测试
#####调用it函数，和调用describe函数差不多，也是传两个参数，一个字符串，描述你这个测试的目的，一个回调函数，包含测试代码。
2.1 测试javascript对象、变量是否满足某种条件，比如变量的类型，数值是否满足某种条件；方法是否被正确调用，方法调用的结果是否符合预期等。具体参考[jasmi*machter](https://jasmine.github.io/api/2.8/matchers.html)

>   ps:这个部分的代码不是伪代码，基于两个类：ContactBook：通讯录，Contact：联系人
```javascript
var ContactBook = function() {
     this.contacts = [];
}

ContactBook .prototype.addContact = function(contact) {
    this.contacts.push(contact;);
}

ContactBook .prototype.getContact= function(index) {
        return this.contacts[index];
}

var Contact = function() {
    // 不要怀疑，我就是一个空函数
}

```
```javascript
describe('RSS Feeds', function() {
        var contactBook，contact；
       
        // 测试向通讯录中添加联系人是否成功
        beforeEach(function() {           
               contactBook = new ContactBook(),
               contact = new Contact();
        });
/* 这是我们的第一个测试 */
it('are defined', function() {
       contactBook.addContact(contact);
       // 测试是否添加成功，并且添加的联系人对象是否是我们创建的对象
       expect(contactBook.getContact(0)).toBe(contact);
       // 测试addContact方法是否被调用，并且方法参数是contact
       expect(contactbook.addContact).toHaveBeenCalledWith(contact);
});
});

```

2.2 测试dom对象是否满足某种条件，比如是否具有某个css类
```javascript
    /*  写一个叫做 "The menu" 的测试用例 */
    describe('The menu',function() {
        var $body, $menuIcon;

        beforeEach(function() {
            $body = $('body');
            $menuIcon = $('.menu-icon-link');
        });
        /* 
        * 写一个测试用例保证菜单元素默认是隐藏的。你需要分析 html 和 css
        * 来搞清楚我们是怎么实现隐藏/展示菜单元素的。
        */
        it('The menu should be closed by default',function() {
            expect($body.hasClass('menu-hidden')).toBeTruthy();
        });

        /* 
          * 写一个测试用例保证当菜单图标被点击的时候菜单会切换可见状态。这个
          * 测试应该包含两个 expectation ： 党点击图标的时候菜单是否显示，
          * 再次点击的时候是否隐藏。
          */
        it('The menu should be switched with the click of the button',function() {
            $menuIcon.trigger('click');
            expect($body.hasClass('menu-hidden')).toBeFalsy();
            $menuIcon.trigger('click');
            expect($body.hasClass('menu-hidden')).toBeTruthy();
        });
    });
```

2.3 测试dom点击事件，点击前后是否符合预期
```javascript
/* 写一个叫做 "The menu" 的测试用例 */
describe('The menu',function() {
var $body, $menuIcon;

beforeEach(function() {
    $body = $('body');
    $menuIcon = $('.menu-icon-link');
});

/*
* 写一个测试用例保证当菜单图标被点击的时候菜单会切换可见状态。这个
* 测试应该包含两个 expectation ： 党点击图标的时候菜单是否显示，
* 再次点击的时候是否隐藏。
*/
it('The menu should be switched with the click of the button',function() {
      $menuIcon.trigger('click');
      expect($body.hasClass('menu-hidden')).toBeFalsy();
      $menuIcon.trigger('click');
      expect($body.hasClass('menu-hidden')).toBeTruthy();
});
});
```

2.4 测试异步调用
```javascript
/* 写一个叫做 "New Feed Selection" 的测试用例，（必要条件一）此处的loadFeed是向服务器发送请求的异步函数 */
    describe('New Feed Selection',function() {
      var content;
      beforeEach(function(done) {// （必要条件二）测试异步调用时 done 是jasmine内置的函数，测试异步调用时该参数必传
          content = $('.feed').html();
          for(var i = allFeeds.length -1; i >=0; i-- ) {
            loadFeed(3, done);// （必要条件三）异步函数loadFeed在请求成功的回调函数里调用done函数
          }        
      });
        /* 
        * 写一个测试保证当用 loadFeed 函数加载一个新源的时候内容会真的改变。
        * 记住，loadFeed() 函数是异步的。
        */
        it('content in container with class feed should be changed when function loadFeed works',function(done) {
            expect(content).not.toBe($('.feed').html());
            done(); // （必要条件四）此处必须调用该函数
        }); 
  });
```
>总结：测试异步调用的注意事项
1，测试异步调用时必须要配合使用beforeEach和it；
2，有四个必要条件，如上面所示。







































# 项目预览

在这个项目中，你会得到一个基于 web 的用来读取 rss 源的应用。最开始的这个项目的开发者意识到了测试的价值，他们也已经把 [Jasmine](http://jasmine.github.io) 包含在了项目之中，而且甚至已经开始写他们第一个测试用例。但是不幸的是，他们决定去创建一个他们自己的公司，所以我们现在拿到的是一个缺乏完整测试用例的应用，这样是你会参与到这个项目的原因。

## 项目目的

测试是开发过程中一个很重要的部分，很多组织把一个标准的开发过程称之为测试驱动开发。意思就是开发人员在他们开始着手编写应用代码之前先写好测试用例。当然这个时候所有的测试用例都是通不过的，然后他们就开始编写应用代码使测试全部通过。

不管你是在一个推崇测试驱动开发的组织，或者是一个编写测试只是为了防止未来的开发中出现与已有代码冲突的 bug 的团队工作，测试都是我们要掌握的一项重要技能。

## 我会学到什么

你会学到怎么使用 Jasmine 来给已经写好的应用编写一定数量的测用例。这些测试用例既要测试深层次的商业逻辑，也要测试时间处理和 DOM 操作。

## 这对我的职业有何帮助？

* 编写测试需要分析应用中诸如 html , css , javascript 之类的各个层面。当你换了团队工作或者加入到一个新的公司，这项技能尤其重要（译者注：指阅读新的项目代码的能力）
* 长期编写测试会让你拥有不需要手动编写测试去测试所有的功能就能快速分析新的代码是否和已知代码冲突的能力。


# 我如何完成这个项目

查看订阅阅读器项目的[评审标准](https://review.udacity.com/#!/projects/3442558598/rubric)

1. 上一下 javascript Testing [课程](https://www.udacity.com/course/ud549)
2. 下载[必要的项目资源](http://github.com/udacity/frontend-nanodegree-feedreader))
3. 在浏览器里面查看一下应用的功能
4. 查看项目的 HTMl (**./index.html**), CSS (**./css/style.css**) 和 JavaScript (**./js/app.js**) 文件来对项目的工作原理有一个基本的了解。
5. 查看 Jasmine spec 文件 **./jasmine/spec/feedreader.js** 然后翻阅阅读 [Jasmine 文档](http://jasmine.github.io)。
6. 编辑 **./js/app.js** 里面的 `allFeeds` 变量使给出的测试通不过，然后观察Jasmine是怎么展示你应用的错误信息的。
7. 将 `allFeeds` 变量返回给一个短暂的状态。（待修改）
8. 编写一个测试遍历 allFeeds 对象里面的所有的源来保证有链接字段而且链接不是空的。
9. 编写一个测试遍历 allFeeds 对象里面的所有的源来保证有名字字段而且不是空的。
10. 写一个叫做 `"The menu"` 的测试用例
11. 写一个测试用例保证菜单元素默认是隐藏的。你需要分析 html 和 css 来搞清楚我们是怎么实现隐藏/展示菜单元素的。
12. 写一个测试用例保证当菜单图标被点击的时候菜单会切换可见状态。这个测试应该包含两个 expectation ： 党点击图标的时候菜单是否显示，再次点击的时候是否隐藏。
13. 写一个叫做 `"Initial Entries"` 的测试用例
14. 写一个测试保证 `loadFeed` 函数被调用而且工作正常，即在 `.feed` 容器元素里面至少有一个 `.entry` 的元素。
15. 写一个叫做 `"New Feed Selection"` 的测试用例
16. 写一个测试保证当用 `loadFeed` 函数加载一个新源的时候内容会真的改变。
17. 每个测试都不应该依赖别的测试的结果。
18. 回调函数应该用来保证在测试运行之前源已经被加载。
19. 实现未定义变量和数组越界的错误处理。
20. 当完成所有任务的时候，所有的测试也应该通过。
21. 写一个 README 文件来详细说明运行应用的步骤。如果你已经添加了额外的测试（来提高测试覆盖率），请提供文档说明这些未来的功能点是什么和你编写的测试在检查什么。
