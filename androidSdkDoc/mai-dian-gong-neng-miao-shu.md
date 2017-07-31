
## 埋点分为可视化埋点和全埋点
### 可视化埋点即连接 ‘幽灵分析系统’ 中 ‘幽灵追踪’ 功能进行圈选埋点
### 全埋点则是在没有进行可视化圈选的情况下进行默认事件绑定，目前支持点击和文本变化监听
### 几乎可以支持所有控件的埋点包括非Activity的window。
# 原理
无论可视化还是全埋点，其根本原理就是找到所选控件在屏幕中的实际位置，而这个觉得位置则是根据path来确定

```json
{
    "$path": [
        {
            "prefix": "shortest",
            "index": 0,
            "id": 16908290
        },
        {
            "view_class": "com.bricks.widgets.base.BaseContextView",
            "index": 0
        },
        {
            "sp_id_name": "dl_filter",
            "index": 0
        },
        {
            "view_class": "android.widget.LinearLayout",
            "index": 0
        },
        {
            "sp_id_name": "bottom_navigation_bar",
            "index": 0
        },
        {
            "sp_id_name": "bottom_navigation_bar_container",
            "index": 0
        },
        {
            "sp_id_name": "bottom_navigation_bar_item_container",
            "index": 0
        },
        {
            "view_class": "com.nwd.bottomnavigationbar.NwdShiftingBottomNavigationTab",
            "index": 2
        }
    ]
}
```
path根据4个属性来确定（index,tag,id,contentDescription）
如果path一直圈选则都会选中
# 视图复用（重点）
* 场景1.同一个activity用一份xml但是页面逻辑却不同，通常手动埋点，会通过逻辑进行判断。
* 场景2.list控件AdapterView RecylerView这些视图都是复用的，itmeView几乎不可能通过在父容器的index来精确定位其位置，所以此类view的index全部取消
* 场景3.一个页面通过某些逻辑，动态加载了一个视图，这个视图都是同一个份XML或者是一个自定义控件，比如一个页面有多个可点击的视图，点击一个弹出一个新的ListViewDialog，而这些数据需要分开统计，但是这些加载进来的视图唯一的区别仅仅只是展示的数据不同，所以我们必须手动进行区分，因为specter只知道如何去定位一个view的path并不能知道业务逻辑，需要开发者自己来做区分。
![](device-2017-07-31-113339.png)
![](device-2017-07-31-113816.png)
![](device-2017-07-31-113833.png)

如图所示，我们看到了两个弹框，这两个弹框都是一个普通的window，列表上每一个item的path都是一样的
![](7a2808a9-ad59-4df1-bd2f-19636b37f8ff.png)

```json
                "path": [
                                {
                                    "prefix": "shortest",
                                    "index": 0
                                },
                                {
                                    "view_class": "android.widget.PopupWindow.PopupBackgroundView",
                                    "index": 0
                                },
                                {
                                    "view_class": "android.widget.ListPopupWindow.DropDownListView",
                                    "index": 0,
                                },
                                {
                                    "sp_id_name": "android:text1",
                                }
                            ]
```













