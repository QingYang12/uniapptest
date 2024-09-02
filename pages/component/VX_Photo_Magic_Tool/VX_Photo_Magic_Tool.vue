<template>
    <view>
        <page-head :title="title"></page-head>
        <view class="uni-padding-wrap uni-common-mt">
			<view class="title" style="display: flex; justify-content: center;color:#00FF00">状态：等待上传</view>
			<view class="uni-title">
				请上传 <text>\n本地图片</text>
			</view>
			<view class="uni-center" style="background:#FFFFFF; font-size:0;">
				<image class="image" mode="widthFix" src="../../../static/upload.png" />
			</view>
            <button type="primary">此处上传要修改的图片</button>
			<view class="title" style="display: flex; justify-content: center;color:#0000FF">图片中解析出的对话会填于下方</view>
			<view class="title" style="display: flex; justify-content: center;color:#0000FF">修改改后点击提交即可重新生成图片</view>
			<view class="title" style="display: flex; justify-content: center;color:#0000FF">生成后会出现图片预览，点击下载即可下载到本地</view>
			<view class="title" style="color: #555555;margin: 10px;">A的所有对话内容：</view>
			<view class="uni-input-wrapper">
			    <input class="uni-input" focus placeholder="要修改的语句A:1" />
			</view>
			<view class="uni-input-wrapper">
			    <input class="uni-input" focus placeholder="要修改的语句A:2" />
			</view>
			<view class="uni-input-wrapper">
			    <input class="uni-input" focus placeholder="要修改的语句A:3" />
			</view>
			<view class="title" style="color: #555555;margin: 10px;" >B的所有对话内容：</view>
			<view class="uni-input-wrapper">
			    <input class="uni-input" focus placeholder="要修改的语句B:1" />
			</view>
			<view class="uni-input-wrapper">
			    <input class="uni-input" focus placeholder="要修改的语句B:2" />
			</view>
			<view class="uni-input-wrapper">
			    <input class="uni-input" focus placeholder="要修改的语句B:3" />
			</view>
			<button type="primary">提交</button>
			<view class="title" style="color: #00FF00;">生成预览：</view>
			<view class="uni-center" style="background:#FFFFFF; font-size:0;">
				<image class="image" mode="widthFix" src="../../../static/status.png" />
			</view>
			<button type="primary" style="backgroundColor:#1AAD19;">下载</button>
        </view>
    </view>
</template>
<script>
    export default {
        data() {
            return {
                title: 'VX修图神器',
                loading: false
            }
        },
        onLoad() {
            this._timer = null;
        },
        onShow() {
            this.clearTimer();
            this._timer = setTimeout(() => {
                this.loading = true;
            }, 300)
        },
        onUnload() {
            this.clearTimer();
            this.loading = false;
        },
        methods: {
            openTypeError(error) {
                console.error('open-type error:', error);
            },
            clearTimer() {
                if (this._timer != null) {
                    clearTimeout(this._timer);
                }
            }
        }
    }
</script>

<style>
    button {
        margin-top: 30rpx;
        margin-bottom: 30rpx;
    }

    .button-sp-area {
        margin: 0 auto;
        width: 60%;
    }

    .mini-btn {
        margin-right: 10rpx;
    }
</style>
