
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
