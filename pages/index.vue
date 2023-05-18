<template>
  <view class="container">
  	<view class="content">
  	  <uni-search-bar @confirm="search" v-model="searchValue" bgColor="#fff" clearButton="node" cancelButton="node" cancel-text="查询" placeholder="请输入完整的标识代码,如:BJ01/0801/10000...">
		<template v-slot:searchIcon>
	  		<span></span>
	  	</template>
	  </uni-search-bar>
	  
  	</view>
	<view class="btn">
		<button style="background-color: #266783;color: #fff;border-radius: 0;" size="mini" @click="switchBtn">查询</button>
	</view>
	<view class="banner">
		<view class="bannerList">
			<view class="bannerTitle">
				{{title}}
			</view>
			<view class="lists" v-for="item in items">
				<view class="listsLeft">
					{{item.k}}：
				</view>
				<view class="listsRight">
					{{item.v}}
				</view>
			</view>
		</view>
		<view class="footer">
			<uni-table ref="table" border>
				<uni-tr v-for="(item, index) in tableData" :key="index">
					<uni-td width="80">
						<view class="name">{{ item.name }}</view>
					</uni-td>
					<uni-td align="left">
						<view class="tableText">{{ item.text }}</view>
					</uni-td>
				</uni-tr>
			</uni-table>
		</view>
	</view>
	
  </view>
</template>

<script>
	import { getTrace } from '@/api/system/user.js'
  export default {
	  data() {
	  			return {
	  				searchValue: '',
					baseFormData: {
						name: '',
					},
					items: [
						// { lable: '制品名称',value: '片仔癀' },
						// { lable: '制品英文名',value: 'Pien Tze Huang' },
						// { lable: '标识代码',value: 'Fj11/21/xkykbkk3ky' },
						// { lable: '标识用户',value: '漳州片仔癀药业股份有限公司' },
						// { lable: '制品规格',value: '待填' },
						// { lable: '原材料',value: '麝香(Moschus)' },
						// { lable: '批准文件',value: '（闽动植）林许（2021）78号' },
						// { lable: '批准时间',value: '2021' },
						// { lable: '核发单位',value: '国家林业和草原局全国野生动植物研究与发展中心' },
						// { lable: '备注',value: '产品拉丁名后面的/B表示人工饲养；/W表示野生；/N表示来源未知；/etc表示“等”。' },
					],
					title:'野生动物及其制品溯源信息',
					tableData: [{name:'标识服务',text:'国家林业和草原局全国野生动植物研究与发展中心\n010-62888966'},{name:'真伪鉴定',text:'国家林业和草原局野生动植物检测中心\n0451-82190626'},{name:'违法举报',text:'010-62888606\n邮箱savewildlife@caf.ac.cn'}],
}
	  		},
	  		methods: {
	  			search(res) {
					getTrace(res.value).then(resp => {
					  this.items = resp.data.product.attrs
					})
	  			},
				switchBtn() {
					this.search({value:this.searchValue})
				},
	  		},
			
	  		onBackPress() {
	  			// #ifdef APP-PLUS
	  			plus.key.hideSoftKeybord();
	  			// #endif
	  		},
    onLoad: function(option) {
		console.log(option)
		if(option.code){
			this.searchValue = option.code
			this.search({value:option.code})
		}
		
    },
	onShow:function(option){
		console.log(getCurrentPages())
	}
  }
</script>

<style lang="scss">
  .content {
    display: flex;
    flex-direction: column;
    // align-items: center;
    // justify-content: start;
  }
  .uni-searchbar{
	  width: 88%;
	  padding-top: 3px;
	  padding-left: 3px;
	  padding-bottom: 0;
	/deep/ .uni-searchbar__box{
		height: 32px;
		border-radius: 0 !important; 
		border: 1px solid #e5e5e5;
	} 
  }
  
  .btn {
	  width: 20%;
	  margin-top: 1px;
	}
	.banner{
		width: 100%;
		padding: 10px;
		font-size: 14px;
		color: #333;
		background: url(../static/images/checkbg.png) no-repeat;
		background-size: 100%;
	}
		
	.bannerTitle{
		font-size: 15px;
		color: #34a471;
		font-weight: bold;
		margin-bottom: 6px;
		margin-left: 6px;
	}
	.lists{
		display: flex;
		align-items: center;
		padding: 3px;
		margin-left: 6px;
	}
	.listsLeft{
		width: 28%;
	}
	.listsRight{
		width: 72%;
	}
	.footer{
		margin-top: 10px;
	}
	.footer /deep/ .uni-table{
		background: none !important;
	}
	.footer /deep/ .table--border{
		border-color: #999 !important;
	}
	.uni-table-td{
		padding: 4px 10px;
	}
	.name{
		font-size: 14px;
		color: #333;
	}
	.tableText{
		white-space: pre-wrap;
		word-break: break-word;
		font-size: 14px;
		color: #333;
		line-height: 18px;
	}
  	.search-result-text {
  		text-align: center;
  		font-size: 14px;
  		color:#666;
  	}
  
  	.example-body {
  		/* #ifndef APP-NVUE */
  		display: block;
  		/* #endif */
  		padding: 0px;
  	}
  
  	.uni-mt-10 {
  		margin-top: 10px;
  	}
</style>
