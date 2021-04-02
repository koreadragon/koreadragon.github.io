---
layout: post
title:  "masonry update布局时的动画"
date:   2021-04-02 14:03:00 +0800
categories: jekyll update
---
参照以下代码
{% highlight swift %}
[UIView animateWithDuration:0.25 animations:^{
    [_sonview mas_updateConstraints:^(MASConstraintMaker *make) {
        make.width.mas_equalTo(arc4random() % 100 + 20);
    }];
}];
{% endhighlight %}
默认只会调用setNeedsLayout方法，并将其标记为“需要刷新”，但不立即刷新，在系统runloop的下一个周期自动调用layoutSubviews。所以在UIView的动画代码块里，由于视图没有更改，所以没有动画效果。
但是如果我把代码改成下面这样的,
{% highlight swift %}
[UIView animateWithDuration:0.25 animations:^{
    [_sonview mas_updateConstraints:^(MASConstraintMaker *make) {
        make.width.mas_equalTo(arc4random() % 100 + 20);
    }];
    [self.view layoutIfNeeded];
}];
{% endhighlight %}
也就是在动画代码块的最后，让父视图调用layoutIfNeeded，立即刷新父控件里的所有子控件，对子控件重新布局。由于此布局同步发生，所以在UIview的动画代码块里，视图进行了更改，就有了动画效果。



