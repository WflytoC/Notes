Android中实现序列化有两个选择：一是实现`Serializable`接口（是JavaSE本身就支持的），一是实现`Parcelable`接口（是Android特有功能，效率比实现`Serializable`接口高效，可用于`Intent`数据传递，也可以用于进程间通信（IPC））。实现`Serializable`接口非常简单，声明一下就可以了，而实现`Parcelable`接口稍微复杂一些，但效率更高，推荐用这种方法提高性能。

注：Android中Intent传递对象有两种方法：一是`Bundle.putSerializable(Key，Object)`，另一种是`Bundle.putParcelable(Key，Object)`。当然这些Object是有一定的条件的，前者是实现了Serializable接口，而后者是实现了`Parcelable`接口。

`Parcelable`接口的定义：

```
public interface Parcelable 
{
    //内容描述接口，基本不用管
    public int describeContents();
    
    //写入接口函数，打包
    public void writeToParcel(Parcel dest, int flags);
    
    //读取接口，目的是要从Parcel中构造一个实现了Parcelable的类的实例处理。因为实现类在这里还是不可知的，所以需要用到模板的方式，继承类名通过模板参数传入
    //为了能够实现模板参数的传入，这里定义Creator嵌入接口,内含两个接口函数分别返回单个和多个继承类实例
    public interface Creator<T> 
    {
           public T createFromParcel(Parcel source);
           public T[] newArray(int size);
    }
}
```

实现该接口的例子：

```
public class MyParcelable implements Parcelable 
{
     private int mData;

     public int describeContents() 
     {
         return 0;
     }

     public void writeToParcel(Parcel out, int flags) 
     {
         out.writeInt(mData);
     }

     public static final Parcelable.Creator<MyParcelable> CREATOR = new Parcelable.Creator<MyParcelable>() 
     {
         public MyParcelable createFromParcel(Parcel in) 
         {
             return new MyParcelable(in);
         }

         public MyParcelable[] newArray(int size) 
         {
             return new MyParcelable[size];
         }
     };
     
     private MyParcelable(Parcel in) 
     {
         mData = in.readInt();
     }
 }
```