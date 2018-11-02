#类通讯录选择器
> - 实现思路：
>   - 1、右侧sidebar通过触摸事件得到当前滑动到的 ‘data-ch’ 位置，并通知父组件滑动到相应位置
>   - 2、main区域各个子模块的详细数据展示，通过插槽slot提供给用户，用户可自行DIY
>   - 3、同时对于子模块，提供了 ‘line-text’ ，‘line-text-img’， ‘grid-text’， ‘grid-img-text’ 四种主题，对于grid类型的主题，还提供属性 ‘col-size’ 用来设置每一行展示几列

grid-img和grid-img-text默认设计成5列，用flex实现