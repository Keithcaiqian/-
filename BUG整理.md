# input type=file BUG

```
//用自定义盒子触发input
$("#importFood").on("click", function () {
    $("#uploadExcelFood").trigger("click");
});
//监听input的改变文件的时候一定要用下面这种事件代理的方式，因为改变type的时候，监听将会失效
$("#menuSettingsPage").on("input",'#uploadExcelFood', function () {
    importFood();
});
//改变type，为了解决同一文件不能上传的问题
impotFood(){
	let uploadDom = $('#uploadExcelFood');
    uploadDom.prop('type','text');
    uploadDom.prop('type','file');
}
```

