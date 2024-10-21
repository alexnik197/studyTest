<template>
	<li v-for="block in taskList">
		<h1 v-if="block.title == 0">{{ block.info }}</h1>
		<h2 v-if="block.title == 1">{{ block.info }}</h2>
		<div class="current-task" v-if="block.title == 2 || block.title == 3">
			<p>{{ block.info.description }}</p>
			<div class="choice-list">
				<div v-for="task in block.info.tasks" class="choice">
					<input v-if="block.title == 2" type="radio" name="task">
					<input v-if="block.title == 3" type="checkbox" name="task">
					<span>{{ task.description }}</span>
				</div>
			</div>
		</div>
		<div class="choice-write" v-if="block.title == 4">
			<p>{{ block.info.description }}</p>
			<input type="text">
		</div>
	</li>
</template>

<script>
export default {
	props: ['taskList'],

};
</script>

<style scoped>
.current-task {
	display: flex;
	flex-direction: column;
	gap: 1rem;
}

.choice {
	display: flex;
	justify-content: center;
	align-items: center;
	flex-direction: row;
	gap: 3px;
}

.choice-write {
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	gap: 1rem;
}

.choice-list {
	display: grid;
	grid-template-columns: 1fr 1fr;
	grid-template-rows: 1fr 1fr;
}

h1 {
	font-size: 24px;
}

h2 {
	font-size: 18px;
}

p {
	text-align: center;
	font-size: 18px;
}

li {
	display: flex;
	justify-content: center;
	align-items: center;
	flex-direction: column;
}
</style>