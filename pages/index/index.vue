<template>
	<view class="container">
		<view class="title">幸运转盘</view>
		<view class="wheel-container">
			<view class="wheel-wrapper" :style="{ transform: `rotate(${rotation}deg)` }">
				<view class="wheel">
					<view class="wheel-item item-1">
						<text class="name">参与者A</text>
					</view>
					<view class="wheel-item item-2">
						<text class="name">参与者B</text>
					</view>
					<view class="wheel-item item-3">
						<text class="name">参与者C</text>
					</view>
					<view class="wheel-item item-4">
						<text class="name">参与者D</text>
					</view>
				</view>
			</view>
			<view class="pointer"></view>
		</view>
		<view class="result" v-if="result">
			<text class="result-text">恭喜：{{ result }}！</text>
		</view>
		<button class="spin-btn" @click="spin" :disabled="spinning">开始转盘</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				rotation: 0,
				spinning: false,
				result: '',
				names: ['参与者A', '参与者B', '参与者C', '参与者D']
			}
		},
		methods: {
			spin() {
				if (this.spinning) return;
				
				this.spinning = true;
				this.result = '';
				
				// 随机生成旋转角度（至少转3圈，最多转8圈）
				const baseRotation = 360 * (3 + Math.random() * 5);
				// 随机选择一个区域（0-3对应4个人）
				const selectedIndex = Math.floor(Math.random() * 4);
				// 每个区域占90度，计算目标角度
				const targetAngle = selectedIndex * 90;
				// 总旋转角度
				const totalRotation = this.rotation + baseRotation + (360 - targetAngle);
				
				this.rotation = totalRotation;
				
				// 计算旋转时间（根据角度）
				const duration = 3000 + Math.random() * 1000; // 3-4秒
				
				// 旋转结束后显示结果
				setTimeout(() => {
					this.result = this.names[selectedIndex];
					this.spinning = false;
				}, duration);
			}
		}
	}
</script>

<style scoped>
	.container {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		min-height: 100vh;
		background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
		padding: 40rpx;
	}
	
	.title {
		font-size: 48rpx;
		font-weight: bold;
		color: #fff;
		margin-bottom: 60rpx;
		text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
	}
	
	.wheel-container {
		position: relative;
		width: 600rpx;
		height: 600rpx;
		margin-bottom: 60rpx;
	}
	
	.wheel-wrapper {
		width: 100%;
		height: 100%;
		transition: transform 3s cubic-bezier(0.17, 0.67, 0.12, 0.99);
	}
	
	.wheel {
		width: 100%;
		height: 100%;
		border-radius: 50%;
		position: relative;
		overflow: hidden;
		box-shadow: 0 0 30rpx rgba(0,0,0,0.3);
		border: 8rpx solid #fff;
	}
	
	.wheel-item {
		position: absolute;
		width: 50%;
		height: 50%;
		transform-origin: 100% 100%;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	
	.item-1 {
		top: 0;
		left: 0;
		background: linear-gradient(135deg, #ff6b6b 0%, #ee5a6f 100%);
		transform: rotate(0deg);
	}
	
	.item-2 {
		top: 0;
		right: 0;
		background: linear-gradient(135deg, #4ecdc4 0%, #44a08d 100%);
		transform: rotate(90deg);
	}
	
	.item-3 {
		bottom: 0;
		right: 0;
		background: linear-gradient(135deg, #ffe66d 0%, #ffd93d 100%);
		transform: rotate(180deg);
	}
	
	.item-4 {
		bottom: 0;
		left: 0;
		background: linear-gradient(135deg, #a8edea 0%, #fed6e3 100%);
		transform: rotate(270deg);
	}
	
	.name {
		font-size: 36rpx;
		font-weight: bold;
		color: #fff;
		text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
		transform: rotate(-45deg);
		white-space: nowrap;
	}
	
	.pointer {
		position: absolute;
		top: -20rpx;
		left: 50%;
		transform: translateX(-50%);
		width: 0;
		height: 0;
		border-left: 20rpx solid transparent;
		border-right: 20rpx solid transparent;
		border-top: 40rpx solid #fff;
		z-index: 10;
		filter: drop-shadow(0 2px 4px rgba(0,0,0,0.3));
	}
	
	.result {
		margin: 40rpx 0;
		padding: 30rpx 60rpx;
		background: rgba(255,255,255,0.15);
		border-radius: 20rpx;
	}
	.result-text { color: #fff; font-size: 36rpx; }
	.spin-btn { margin-top: 20rpx; }
</style>
