`Popover`可以是模态窗口，阻止用户与界面的其它地方交互；或者，当用户点击`Popover`外面时，它就会消失。

iOS8及以后的`Popover`，实际上是一种`presented view controller`(`modalPresentationStyle`设置成`Popover`)

####显示在中间，没有箭头

```
let vc = SNowController()

let vcNav = UINavigationController(rootViewController: vc)

vcNav.modalPresentationStyle = .FormSheet

vcNav.modalTransitionStyle = .CoverVertical

self.presentViewController(vcNav, animated: true, completion: nil)
```
####带有箭头

```
    let vc = MyViewController()
    
    vc.modalPresentationStyle = .Popover
    
    self.presentViewController(vc, animated: true, completion: nil)
   
    if let pop = vc.popoverPresentationController {
        let v = sender as! UIView
        pop.sourceView = v
        pop.sourceRect = v.bounds
    }
```