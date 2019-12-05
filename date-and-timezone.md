### 处理时间问题时一些需要注意的地方

#### 谨慎使用Date.getXXXX()，尽量使用Date.getUTCXXXX()

`javascript`提供了`Date`对象用来处理时间相关的task，其中有取出当前所在年月日时分秒的方法。它们十分便利，但同时也暗藏杀机。

`Date.getXXXX()`取出的是浏览器所在时区的时间戳数据，因此会随着系统时区的变化而返回不同的结果。如果没有考虑到这个问题测试不足的话，就会导致组合出错误的请求，显示出错误的数据等情况。

而使用不受时区影响的`Date.getUTCXXXX()`方法则可以避免上面的问题，因为只要时刻是同一的，那么无论在世界的各个角落，调用这个方法都可以得到同样的结果。

#### jest编写时间相关测试用例踩的坑

现在的前端项目使用`jest`编写测试用例，使用`gitlab CI`进行持续集成（lint / test / deploy），在调试时间测试用例时遇到了这样的现象：
1. 浏览器运行的结果和`npm test`的结果不一样
2. 本地`npm test`的结果和`gitlab CI`里`npm test`的结果不一样

经调查发现`jest`拥有默认的时区（即`UTC`），这导致上述的时区敏感方法都会默认产生`UTC`时区的结果，所以导致和浏览器结果不同。如果想要进行时区相关测试，就需要使用到能够mock时区的方法。

`jest`如何mock时区网上有很多方案，其中有一个高赞解答是在config file里添加`process.env.TZ=XXX`。经过测试确实可以在本地生效。然而当test跑在gitlab runner里时，结果又不对了。查了查发现gitlab有一个[提交](https://gitlab.com/gitlab-org/gitlab-foss/merge_requests/27738/diffs "提交")把默认时区设置成了`GMT`，就是为了解决和`jest`默认时区不一致导致的问题。然而这又给时区相关测试带来了新的难题。

尝试各种方法修改runner时区设置未果之后，最终还是使用了开源库`timezone-mock`来解决mock时区的问题。

#### 捣乱的夏令时

以下是一段测试代码：

```
it('can deal with timezone', () => {
	timezone_mock.register('US/Pacific')
	expect([
		new Date('2019-07-01T04:23:57').getHours(),
		new Date('2019-01-01T04:23:57').getHours(),
		new Date('2019-07-01T04:23:57-0800').getHours(),
		new Date('2019-01-01T04:23:57-0800').getHours()
	]).toEqual([4, 4, 4, 4])
	timezone_mock.unregister()
})
```
结果是：

[![Timezone test result](https://github.com/Elinia/Notes/blob/master/Screenshot%20from%202019-12-05%2017-18-12.png?raw=true "Timezone test result")](https://github.com/Elinia/Notes/blob/master/Screenshot%20from%202019-12-05%2017-18-12.png?raw=true "Timezone test result")

可以看到测试用例中出了一个异类，`new Date('2019-07-01T04:23:57-0800').getHours()`得到的结果是`5`而不是`4`。

原因是`US/Pacific`时区(`Etc/GMT+8`也是如此)会在[每年三月的第二个星期日凌晨二时到每年十一月的第一个星期日凌晨二时之间使用夏令时](https://zh.wikipedia.org/wiki/%E5%A4%AA%E5%B9%B3%E6%B4%8B%E6%97%B6%E5%8C%BA "每年三月的第二个星期日凌晨二时到每年十一月的第一个星期日凌晨二时之间使用夏令时")，夏令时使用`UTC-7`而非夏令时才使用`UTC-8`。

所以当使用或编写时区敏感方法时，千万要考虑到夏令时的影响。比如避免直接在`年/月/日/时`上做`offset`等操作，以及在使用`ISO 8601`时刻表示法编写测试用例的时候注意时区的选择。
