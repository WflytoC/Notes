iOS动画划分为：`视图动画`和`层动画`

简单的动画：

```
UIView.animateWithDuration(_:animations:) 
```
-----------------------------------------



`CAMediaTiming`

```

public protocol CAMediaTiming {
    
    public var beginTime: CFTimeInterval { get set }
        
    public var timeOffset: CFTimeInterval { get set }

    public var repeatDuration: CFTimeInterval { get set }

    public var autoreverses: Bool { get set }
    
    public var fillMode: String { get set }
}
```

------------------------------------------


`CAAnimation`


```
public class CAAnimation : NSObject, NSCoding, NSCopying, CAMediaTiming, CAAction {

    public class func defaultValueForKey(key: String) -> AnyObject?
    public func shouldArchiveValueForKey(key: String) -> Bool
    
    public var timingFunction: CAMediaTimingFunction?
    
    public var delegate: AnyObject?
    
    public var removedOnCompletion: Bool
}
```


`CAPropertyAnimation`

```
public class CAPropertyAnimation : CAAnimation {
    
    //keyPath指的是需要应用动画的属性
    public convenience init(keyPath path: String?)

    public var keyPath: String?

    public var additive: Bool

    public var cumulative: Bool
    
    public var valueFunction: CAValueFunction?
}
```

`CABasicAnimation`

```
//single-frame
public class CABasicAnimation : CAPropertyAnimation {

	 public var fromValue: AnyObject?
    
    public var toValue: AnyObject?
    
    public var byValue: AnyObject?
}
```

`CAKeyframeAnimation`

```

public class CAKeyframeAnimation : CAPropertyAnimation {

    public var values: [AnyObject]?
    
    public var path: CGPath?

    public var keyTimes: [NSNumber]?
   
    public var timingFunctions: [CAMediaTimingFunction]?

    public var calculationMode: String

    public var tensionValues: [NSNumber]?
    public var continuityValues: [NSNumber]?
    public var biasValues: [NSNumber]?

    public var rotationMode: String?
}
```

`CASpringAnimation`

```

	public class CASpringAnimation : CABasicAnimation {
    
    	public var mass: CGFloat
    
    	public var stiffness: CGFloat
    
    	public var damping: CGFloat

    	public var initialVelocity: CGFloat
    
    	public var settlingDuration: CFTimeInterval { get }

	}
```

`CAMediaTimingFunction`

```
public class CAMediaTimingFunction : NSObject, NSCoding {
    
    public convenience init(name: String)
    
    public init(controlPoints c1x: Float, _ c1y: Float, _ c2x: Float, _ c2y: Float)
    
    public func getControlPointAtIndex(idx: Int, values ptr: UnsafeMutablePointer<Float>)
}
```