<template>
	<div class="page-bar">
		<ul>
			<li v-if="pageIndex!=1"><a @click="pre">上一页</a></li>
			<li v-for="item in items" :class="{active: pageIndex == item}">
				<a @click="go(item)" v-text="item"></a>
			</li>
			<li v-if="pageIndex!=all"><a @click="next">下一页</a></li>
			<li><a href="javascript:;"><i v-text="all"></i>页</a></li>
		</ul>
	</div>
</template>

<script>
export default {
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
			if(this.total > this.pageSize){
				if(this.total % this.pageSize != 0){
					return parseInt(this.total/this.pageSize)+1
				}else{
					return this.total/this.pageSize
				}
			}else{
				return 1
			}
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