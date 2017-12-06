
## 1.	使用
### a)	使用本地的json数据
```
 html
 <input type="text" id="departName" name="departName" class="form-control"/>
 js
 //从后台获取数据
 $.getJSON("<%=request.getContextPath()%>/ztree/findLedgerTreeAjax.do",
         function(result){
            // ztree下拉菜单
            $('#departName').ztreeSelect({
                hidObj: $('#ledgerID'),
                dataSource : result,
                onSelect: function(currentNode){
                    $('#ledgerName').val(currentNode.name);
                }
            });
        });
});
```
### b)	使用远程数据，dataSource为url
```
// ztree下拉菜单
$('#departName').ztreeSelect({
	hidObj: $('#ledgerID'),
	dataSource : '<%=request.getContextPath()%>/ztree/findLedgerTreeAjax.do',
	onSelect: function(currentNode){
		$('#ledgerName').val(currentNode.name);
	}
});
```
### c)	使用远程数据，dataSource为function 此时可以根据页面的其他控件的值来动态获取数据，当需要的条件不成立时，不弹出ztree下拉菜单
```
// ztree下拉菜单
$('#departName').ztreeSelect({
    hidObj: $('#ledgerID'),
    dataSource : function(){
        if(param){
            alert('请先选择条件！')
            return false；
        }
    },
    onSelect: function(currentNode){
        $('#ledgerName').val(currentNode.name);
    }
});
```
### d)	复选下拉菜单，加上checkEnable属性即可，如果需要设置勾选时的父子关联关系，可以设置chkboxType属性格式如下 { 'Y': 'ps', 'N': 'ps' }，不关联可以不用设置 Y 属性定义 checkbox 被勾选后的情况； N 属性定义 checkbox 取消勾选后的情况； "p" 表示操作会影响父级节点； "s" 表示操作会影响子级节点。请注意大小写，不要改变
```
// ztree复选下拉菜单
$('#departName').ztreeSelect({
    hidObj: $('#ledgerID'),
    checkEnable: true,
    dataSource : '<%=request.getContextPath()%>/ztree/findLedgerTreeAjax.do',
    onSelect: function(currentNode){
        $('#ledgerName').val(currentNode.name);
    }
});
```
### e)	明细行中使用ztree
```
// 明细行中的树
$('input[name="detailTree"]').each(function(index, ele) {
    $(this).ztreeSelect({
        width: '300px',
        treeId: $(ele).attr('id') || $(ele).attr('name') + index ,
        hidObj: $('input[name="detailTreeID"]').eq(index),
        checkEnable: true,
         dataSource: function() { // 数据源
             if(!!!$('#zTreeSelect').val()){
                 alert('请先选择下拉菜单！');
                 return false;
             }
          return "<%=request.getContextPath()%>/ztree/findLedgerTreeAjax.do";
         },
         onCheck: function(checkedNode){
            var childNum = "";
            for (var i=0, l=checkedNode.length; i<l; i++) {
                 childNum += checkedNode[i].custom2 + "，";
		     }
             if (childNum.length > 0 ) {
                 childNum = childNum.substring(0, childNum.length-1);
             }
             $('input[name="other"]').eq(index).val(childNum);
         }
        });
    });
```
### f)	左侧树菜单
```
    // 左侧树zTree
	$('#treeDemo').zTreeLeftMenu({
	    keepStatus: true, // 是否保持展开合并状态 
	    checkEnable: true , // 是否显示复选框
	    chkboxType: { 'Y': 'ps', 'N': 'ps' },
	    dataSource: "<%=request.getContextPath()%>/ztree/findLedgerTreeAjax.do", // 数据源
	    beforeCheck: function(currentNode){ // 选中之前的回调
// 	        alert(currentNode.id);
	    }
	});
```
## 2.	参数

|      参数       | 说明                                          		|
| ------------- |:-------------:                                       |
| width         | 下拉菜单宽度，默认为绑定的input的宽度                     |
| hidObj        | 需要赋值的隐藏域对象                                    |
| treeId        | ztree树的Id，明细行时可以传name，单个树的控件不需要传      |
|keepStatus     |是否保存展开闭合状态                                     |
|dataSource     |数据源                                                 |
|beforeSelect   |单选ztree选中节点之前的回调函数，参数为当前节点             |
|onSelect       | 单选ztree选中节点时的回调函数，参数为当前节点              |
|initTree       | 初始化方法，参数为treeObj即为原生的zTree对象，可以尽情的使用原生对象的方法|
|defaultOptionLabel| 使用下拉菜单式添加默认的“--请选择--”或者全部|
|position|下拉框弹出的方向，默认向下，支持'auto'自己计算弹出的方向|
|           	|                                                      |
|checkEnable|是否开启复选                                                |
|chkboxType |复选的关联关系                                              |
|beforeCheck|复选ztree勾选节点之前的回调函数，参数为当前节点                 |
|onCheck|复选ztree勾选节点时的回调函数，参数为所有已勾选的节点                |
