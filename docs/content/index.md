# 首页板块
> 快简易CMS模板分为两个部分,模板静态文件和模板qae后缀文件
````
public/templates/模板名/  //静态文件存放目录
templates/模板名/  //模板qae后缀文件存放目录
````
> 以下为模板qae后缀文件列表
````
├─default //模板文件夹
│      detail.qae //详情页
│      foot.qae //全局底部
│      head.qae //全局头部
│      include.qae //全局包含 引入静态文件
│      index.qae //首页
│      list.qae //列表页
│      page.qae //列表翻页
│      play.qae //播放页
│      search.qae //搜索页
````
> include.qae 静态文件引入如下
````
//静态文件引入函数 qae_asset('模板名/css/xxx.css')
如:<link rel="stylesheet" href="{{qae_asset('default/css/iconfont.css')}}" type="text/css" />
````
> 将include.qae 包含到别的文件中使用如下方法
````
@include('模板名.文件名')
如：
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=EmulateIE10" />
    <meta name="renderer" content="webkit|ie-comp|ie-stand">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">
    <title>{{$__WEBNAME__}}</title>
    <meta name="keywords" content="{{$__SEOKEYWORDS__}}" />
    <meta name="description" content="{{$__SEODESCRIPTION__}}" />
    @include('default.include')
</head>

head.qae  foot.qae  page.qae 等文件的包含一样如此
````
> 以下为相关模板方法调用
````
导航:
 @foreach(qae_nav() as $nav)
         <li  class=""><a href="{{$nav->href}}">{{$nav->title}}</a></li>
  @endforeach

轮播:
  @foreach(qae_carousel('index',10) as $banner)
    <a href="{{$banner->href}}" class="stui-vodlist__thumb" title="{{$banner->title}}" style="background: url({{$banner->image}}) no-repeat; background-position:50% 50%; background-size: cover; padding-top: 40%;"><span class="pic-text text-center">{{$banner->title}}</span></a>
  @endforeach

部分视频:
//qae_video(1,0,'time',0,15)参数解释
1//视频类型ID
0//是否为会员 0非会员  1会员
time //按什么倒叙排序  time:最后更新时间  hot:热度  score:评分
0 第几页 
15 限制个数

@foreach(qae_video(1,0,'time',0,15) as $video)
     <li class="col-md-5 col-sm-4 col-xs-3">
          <div class="stui-vodlist__box">
              <a class="stui-vodlist__thumb img-shadow" href="{{route('qaecmsindex.detail',['type'=>"video",'id'=>$video->id])}}" title="{{$video->title}}">
                  <img src="{{$video->thumbnail}}" alt="{{$video->title}}">
                  <span class="play hidden-xs"></span>
                  <span class="pic-text text-right">{{$video->score}}</span>
              </a>
              <div class="stui-vodlist__detail">
                  <h4 class="title text-overflow" align="center">
                      <a href="{{route('qaecmsindex.detail',['type'=>"video",'id'=>$video->id])}}" title="{{$video->title}}">{{$video->title}}</a></h4>
              </div>
          </div>
    </li>
 @endforeach


//详情地址
{{route('qaecmsindex.detail',['type'=>类型 video/article,'id'=>视频ID])}}

//播放页地址
{{route('qaecmsindex.play',['id'=>视频ID])}}

//获取指定分类下的所有子分类
 qae_class_type("video",1)
   video:分类类型 video/article
       1:父类ID 不填代表获取所有父分类
例:
 @foreach(qae_class_type("video",1) as $cat)
                <li class="col-xs-4"><a href="{{route('qaecmsindex.list',['type'=>'video','class'=>1,'cat'=>$cat->id,'page'=>1,'limit'=>35])}}" target='_self'>{{$cat->name}}</a></li>
  @endforeach

//列表页地址
{{route('qaecmsindex.list',['type'=>'video','class'=>4,'cat'=>$cat->id,'page'=>1,'limit'=>35])}}
  type:类型video/article
  class:父类ID
  cat:子类ID 全部为all
  page:翻页
  limit:每页多少个

//列表页数据变量$list
$list->class //当前父类ID
$list->cat //当前子类ID
$list->page //当前页码
$list->totalpage //当前分类总页码
qae_page($list->page,$list->totalpage,4)//获取当前显示多少个页码

//详情页数据变量$detail
视频详情参数列表:
'title', 'introduction', 'seokey', 'thumbnail', 'sid', 'stid', 'stype', 'lang', 'area', 'year', 'note', 'score', 'actor', 'director', 'shost', 'last', 'content', 'editor'
文章详情参数列表
'title', 'type', 'introduction', 'seokey', 'thumbnail', 'content', 'editor'
调用方式 {{$detail->参数名}}

//视频详情页 播放页播放地址方法:
 @foreach($detail->content as $player=>$playlist)
   $player //播放器名称
   $playlist //播放地址数组
    @foreach($playlist as $play)
        $play//当前播放地址信息
        $detail->id//当前视频详情页视频ID
        $play->playerid //当前视频需要的播放器ID
        $play->js //当前视频集数
        $play->episode //当前集数名称
        {{qae_play_now($detail->id,$play->playerid,$play->js)}} //获取当前直接播放地址
        <li><a href='{{qae_play_now($detail->id,$play->playerid,$play->js)}}'  style="width: 100px" target="_self"><span>{{$play->episode}}</span></a></li>
    @endforeach
 @endforeach
例子请查看:detail.qae 文件

//视频播放页数据变量$detail
视频详情参数列表:
'title', 'introduction', 'seokey', 'thumbnail', 'sid', 'stid', 'stype', 'lang', 'area', 'year', 'note', 'score', 'actor', 'director', 'shost', 'last', 'content', 'editor',type
特别注意:
分类名:$detail->type
下一集:$detail->next
上一集:$detail->prev
当前播放地址:$detail->play

播放页播放地址列表:
例:
 @foreach($detail->content as $player=>$playlist)
            <div class="stui-pannel stui-pannel-bg clearfix">
                <div class="stui-pannel-box">
                    <div class="stui-pannel_hd">
                        <div class="stui-pannel__head bottom-line clearfix">
                            <span class="more text-muted pull-right">{{$player}}</span>
                            <h3 class="title">{{$player}}</h3></div>
                    </div>
                    <div class="stui-pannel_bd col-pd clearfix sw">
                            <ul class="stui-content__playlist column10 clearfix">
                                @foreach($playlist as $play)
                                    <li><a href='{{qae_play_now($detail->id,$play->playerid,$play->js)}}'  class="vplay {{$detail->now==$play->playerid.$play->js?"bs":""}}" style="width: 100px" target="_self"><span>{{$play->episode}}</span></a></li>
                                @endforeach
                            </ul>
                    </div>
                </div>
            </div>
    @endforeach
//播放页同名资源切换
$samesource
@foreach($samesource as $source)
<li><a href="/play/{{$source->id}}">{{$source->title}}</a></li>
@endforeach

//搜索页数据变量:$search
当前搜索内容数量:$search->count
当前搜索内容列表:$search->data
当前搜索关键字:request('wd')

//搜索进入地址:
视频:/search/video/搜索关键字.html
文章:/search/article/搜索关键字.html


//历史记录调用
qae_history(10)
历史记录系统最多记录20个
调用时可以设定个数 调用多少个 默认10个

例子:
@foreach(qae_history(10) as $history)
  <li><a href="{{$history->url}}">{{$history->title}}</li>
@endforeach
````

>留言板进入地址
````
模板调用  {{route('qaecmsindex.comments')}}
````

>会员中心地址
````
模板调用 
用户中心 {{route('qaecmsindex.user')}}
````
