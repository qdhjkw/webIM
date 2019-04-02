<template>
    <div id="index">
        <span>
            <v-icon name="home" class="fa-icon"></v-icon>
			软件梦工场-首页
        </span>
    </div>
</template>

<script>
export default {
    name: "Index"
};
</script>

<style lang="less" scoped>
#index {
    text-align: center;
    padding: 20px;
	// background-color: #aaa;
	
    span {
		font-size: 50px;
		
		.fa-icon {
			width: auto;
			height: 1em;
			max-width: 100%;
			max-height: 100%;
		}
	}
	
	.thx {
		text-align: center;
		font-size: 24px;
		padding: 30px;
		color: #F03141;
		line-height: 1.5;
	}

	p {
		padding: 10px 30px;
		font-size: 22px;
		text-align: left;
	}
}
</style>