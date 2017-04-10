#Unity5.6 使用VideoPlayer的注意事项

### 兼容性

Unity在5.6版本加入了新的VideoPlayer来代替Moive Texture，正好手头有个项目要使用视频播放功能就试了一下。初期版本在安卓上还有些兼容性问题，官方说明暂时只 能完美支持4.4以上，而且还不能从AB中读取。

### 如何指定开始的位置

官方文档中指定开始位置有个坑，直接使用了`videoplayer.frame=xxx`这样的方法。 其实这样是没有任何效果的。需要执行`Prepare()`方法初始化准备播放器之后设置才可以的。

示例代码：

```
// use url
m_videoPlayer.source = VideoSource.Url;
m_videoPlayer.url = "http://www.quirksmode.org/html5/videos/big_buck_bunny.mp4";

m_videoPlayer.skipOnDrop = true;

// 初始化准备
m_videoPlayer.Prepare();

// 检查是否完成初始化
while(!m_videoPlayer.isPrepared)
{
    yield return new WaitForSeconds(1);
    break;
}

// 设置开头是第100帧
m_videoPlayer.frame += 100;

// 开始播放
m_videoPlayer.Play();
```

###全屏播放

如果想要全屏播放时，推荐**Render Mode**设置为`Camera Far Plane | Camera Near Plane`, 这样做有几个好处：

1. 只需要调整target camera的depth属性值即可更改幕布渲染层级
2. 通过**Aspect Ratio**属性，可以设置缩放属性
3. 不像使用Material Override一样，需要一个单独的幕布对象（Mesh Renderer）, 所以也不需要考虑光源的影响。 

> 这个属性是在我写了一个可自适应全屏的Mesh预制件之后才发现的。白瞎了时间...

