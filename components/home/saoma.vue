<template>
  <input type='button' @click='startRecognize' value='创建扫描控件' style="display:none"/>
  <input type='button' @click='startScan' value='开始扫描' style="display:none"/>
  <div id= "bcid"></div>
  <input type='text' id='text' style="display:none"/>
</template>

<script>
  import { Scroller,Checklist,Box,XButton} from 'vux'
  import ajax from 'src/ajax/index.js'
  import encryption from 'src/assets/js/encryption.js'

  var scan = null;
  //扩展API加载完毕后调用onPlusReady回调函数
  document.addEventListener( "plusready", onPlusReady, false );

  // 扩展API加载完毕，现在可以正常调用扩展API
  function onPlusReady() {
    var e = document.getElementById("scan");
    e.removeAttribute( "disabled" );
  }

  scan = new plus.barcode.Barcode('bcid');
  scan.onmarked = onmarked;
  scan.start();

  var dataItem;
  export default {
		name: "checkPublished",
		ready(){
		},
		components: {
	    Scroller,Checklist,Box,XButton
	  },
		data() {
			return {
        dataItem:""
			}
		},
		methods: {
        onmarked( type, result ) {
          var text = '未知: ';
          switch(type){
            case plus.barcode.QR:
            text = 'QR: ';
            break;
            case plus.barcode.EAN13:
            text = 'EAN13: ';
            break;
            case plus.barcode.EAN8:
            text = 'EAN8: ';
            break;
          }

          ajax.post("checkPublished", {
						pageSize: "3",
						pageNum: "1",
						marketName:"bc96731e521811e6987cf8cab858db3f",
            text:text
					}, (status,data) => {
						if(status){
							if(data.length>0){
								mui.toast("获取测试数据成功");
							}else{
								mui.toast("获取测试数据失败");
							}
						}
						this.disablevalue=false;
					},false)

        },
        startRecognize() {
        	scan = new plus.barcode.Barcode('bcid');
        	scan.onmarked = onmarked;
        },
        startScan() {
        	scan.start();
        }
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
*{
	-webkit-user-select: none;
}
html,body{
	margin: 0px;
	padding: 0px;
	height: 100%;
}
#bcid {
	background:#0F0;
	height:480px;
	width:360px;
}
</style>
