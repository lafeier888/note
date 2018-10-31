Felix



# 入门





# 嵌入式使用

[参考](http://felix.apache.org/documentation/subprojects/apache-felix-framework/apache-felix-framework-launching-and-embedding.html)

## **创建框架对象**

```
org.osgi.framework.launch.Framework接口
org.osgi.framework.launch.FrameworkFactory  工厂接口
```

用上面这俩接口的实现类都可以创建框架对象,felix实现了这俩接口

Framework的对应实现为org.apache.felix.framework.Felix

FrameworkFactory的对应实现为org.apache.felix.framework.FrameworkFactory

## **框架配置**

构造框架对象需要一个Configure,可以有

[参考配置](http://felix.apache.org/documentation/subprojects/apache-felix-framework/apache-felix-framework-configuration-properties.html)

```yaml
felix.auto.deploy.dir   bundle目录
felix.auto.deploy.action	在bundle目录发现bundle后的操作
org.osgi.framework.storage  缓存目录
```

## **启动**

- `init()` 初始化框架
- `start()` 启动框架



## **Bundle编写**

实现BundleActivator

```
public class HostActivator implements BundleActivator
{
    private BundleContext m_context = null;

    public void start(BundleContext context)
    {
        m_context = context;
    }

    public void stop(BundleContext context)
    {
        m_context = null;
    }

    public Bundle[] getBundles()
    {
        if (m_context != null)
        {
            return m_context.getBundles();
        }
        return null;
    }
}
```