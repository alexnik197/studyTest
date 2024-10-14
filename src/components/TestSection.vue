<template>
	<li v-for="task in taskList">
		<h1 v-if="task.title == 0">{{ task.info }}</h1>
		<h2 v-if="task.title == 1">{{ task.info }}</h2>
		<div v-if="task.title == 2">
			<p>{{ task.info[0].description }}</p>
			<div class="choice-list">
				<div v-for="task in task.info" class="choice">
					<input type="checkbox">
					<span>{{ task.description }}</span>
				</div>
			</div>
		</div>
	</li>
</template>

<script>
export default {
	props: ['taskList'],

};
</script>

<style scoped>
.choice {
	display: flex;
	align-items: center;
	flex-direction: row;
	gap: 3px;
}

h1 {
	font-size: 24px;
}

h2 {
	font-size: 18px;
}

p {
	font-size: 14px;
}

li {
	display: flex;
	align-items: center;
	flex-direction: column;
}
</style>