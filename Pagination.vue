<template>
	<div class="page-bar">
		<ul>
			<li v-if="pageIndex!=1"><a @click="pre">上一页</a></li>
			<li v-for="item in items" :class="{active: pageIndex == item}">
				<a @click="go(item)" v-text="item"></a>
			</li>
			<li v-if="pageIndex!=all"><a @click="next">下一页</a></li>
			<li class="page-tonum"><input type="text" v-model="toPage"><a href="javascript:;" @click="goNumPage">GO</a></li>
			<li><a href="javascript:;">共<i v-text="all"></i>页</a></li>
		</ul>
	</div>
</template>

<script>
export default {
	data () {
		return {
			toPage: 1
		}
	},
	props: {
		pageIndex: {
			type: Number,
			default: 1
		},
		/*all: {
			type: Number,
			default: 1
		},*/
		total: {
			type: Number,
			default: 1
		},
		pageSize: {
			type: Number,
			default: 20
		}
	},
	computed: {
		all () {
			return window.Math.ceil(this.total/this.pageSize)
		},
		items () {
			let [left, right, ar] = [1, this.all, []]
			if(this.all >= 11){
				if(this.pageIndex > 5 && this.pageIndex < this.all -4){
					[left, right] = [this.pageIndex - 5, this.pageIndex + 4]
				}else{
					if(this.pageIndex <= 5){
						[left, right] = [1, 10]
					}else{
						[left, right] = [this.all-9, this.all]
					}
				}
			}
			while(left <= right){
				ar.push(left)
				left++
			}
			return ar
		}
	},
	methods: {
		pre () {
			this.pageIndex--
			this.$emit('gopage', this.pageIndex)
		},
		next () {
			this.pageIndex++
			this.$emit('gopage', this.pageIndex)
		},
		go (index) {
			if(index != this.pageIndex){
				this.pageIndex = index
				this.$emit('gopage', index)
			}
		},
		goNumPage () {
			if(/(^[1-9][0-9]*$)/.test(this.toPage)){
				if(this.toPage > this.all){
					this.toPage = this.all
				}
				this.go(this.toPage)
			}else{
				this.toPage = this.pageIndex
			}
		}
	}
}
</script>

<style lang="sass">
.page-bar{
	width: 100%;
	padding-top: 20px;
	padding-bottom: 30px;

}
.page-bar ul{
	// max-width: 560px;
	margin-left: 350px;
	overflow: hidden;
}
.page-bar li{
    list-style: none;
    float:left;
}
.page-bar li:first-child>a {
   margin-left: 0px
}
.page-bar a{
    border: 1px solid #ddd;
    text-decoration: none;
    position: relative;
    float: left;
    padding: 6px 12px;
    margin-left: -1px;
    line-height: 1.42857143;
    color: #337ab7;
    pageIndexsor: pointer
}
.page-bar li.page-tonum a{
	padding: 6px;
    border-radius: 6px;
    background-color: #25263E;
    float: none;
    color: whitesmoke;
}
.page-bar li.page-tonum input{
	width: 30px;
	margin-left: 5px;
	margin-right: 5px;
    border: 1px solid #ccc;
    height: 33px;
    text-align: center;
}
.page-bar a:hover{
    background-color: #eee;
}
.page-bar .active a{
    color: #fff;
    pageIndexsor: default;
    background-color: #337ab7;
    border-color: #337ab7;
}
.page-bar i{
    font-style:normal;
    color: #d44950;
    margin: 0px 4px;
    font-size: 12px;
}
</style>