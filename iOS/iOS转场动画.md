自定义转场API：

* 模态`present`和`dismiss`
* 导航`push`和`pop`
* `TabBarController`


自定义转场的步骤：

* 自定义一个遵循的`<UIViewControllerAnimatedTransitioning>`协议的`动画过渡管理`对象，并实现两个必须实现的方法：

		//返回动画事件  
 		- (NSTimeInterval)transitionDuration:(nullable id <UIViewControllerContextTransitioning>)transitionContext;
 		
 		//所有的过渡动画事务都在这个方法里面完成
 		- (void)animateTransition:(id <UIViewControllerContextTransitioning>)transitionContext;
 		
 		
* 自定义一个继承于`UIPercentDrivenInteractiveTransition`的手势过渡管理对象


* 成为相应的代理，实现相应的代理方法，返回我们前两步自定义的对象