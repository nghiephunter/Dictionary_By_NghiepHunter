<template>
	<body>
	<div>
		<span></span>
		<span></span>
		<span></span>
		<span></span>
		Tra từ
	</div>
	</body>	
</template>
<script>
</script>
<style scoped>
	body{
	margin: 0;
	padding: 0;
	display: flex;
	justify-content: center;
	align-items: center;
	min-height: 100vh;
	background-color: #031321;
	font-family: consolas;
}
div{
	position: relative;
	display: inline-block;
	padding: 5px 10px;
	color: #2196F3;
	text-transform: uppercase;
	letter-spacing: 2px;
	text-decoration: none;
	font-size: 24px;
	overflow: hidden;
	transition: 0.2s;
}
div:hover{
	color:#255784;
	background-color: #2196f3;
	box-shadow: 0 0 10px #2196f3, 0 0 40px #2196f3, 0 0 80px #2196f3;
	transition-delay: 1s;
}
div span{
	position: absolute;
	display: block;
}
div span:nth-child(1){
	top: 0;
	left: -100%;
	width: 100%;
	height: 2px;
	background: linear-gradient(90deg,transparent,#2196f3);
}
div:hover span:nth-child(1){
	left: 100%;
	transition: 1s;
}
div span:nth-child(3){
	bottom: 0;
	right: -100%;
	width: 100%;
	height: 2px;
	background: linear-gradient(270deg,transparent,#2196f3);
}
div:hover span:nth-child(3){
	right: 100%;
	transition: 1s;
	transition-delay: 0.5s;
}
div span:nth-child(2){
	top: -100%;
	right: 0;
	width: 2px;
	height: 100%;
	background: linear-gradient(180deg,transparent,#2196f3);
}
div:hover span:nth-child(2){
	top: 100%;
	transition: 1s;
	transition-delay: 0.25s;
}
div span:nth-child(4){
	bottom: -100%;
	left: 0;
	width: 2px;
	height: 100%;
	background: linear-gradient(360deg,transparent,#2196f3);
}
div:hover span:nth-child(4){
	bottom: 100%;
	transition: 1s;
	transition-delay: 0.75s;
}
	
</style>