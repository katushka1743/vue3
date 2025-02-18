<script setup>
import { ref, reactive } from 'vue'


// Получение из компонента mainFile:
// 	messageEl - объект с именем, полом, возрастом, результатом теста и попытками
// 	messageIndex - индекс каждого из пользователей
// 	mesRes - средний результат теста
// 	activeIndex - индекст текущего пользователя
const props = defineProps ({
	messageEl: null, 
	messageIndex: Number,
	mesRes: null,
	activeIndex: Number,
})

// отправка emit
const emit = defineEmits([
	'deleteUser'
])

// Функция для передачи данных о том, какого пользователя необходимо удалить
function deleting() {
	emit('deleteUser', props.messageIndex) 
}
</script>



<template>
	<div class="userCard" :class="{ 'active-card': props.activeIndex !== null && props.messageIndex === props.activeIndex }"> <!-- Применение класса активности, если пользователь выбран и активен -->
		<div>
			<div>Номер регистрации: {{ props.messageIndex + 1 }}</div>
			<div>Имя: {{ props.messageEl.name }}</div>
		</div>
		<div>Пол: {{ props.messageEl.gender }}</div>
		<div>Возраст: {{ props.messageEl.age }}</div>
		<div v-if="props.messageEl.attempts == 0">Вы пока не проходили тест</div>
		<div v-else>
			<div>Средний результат прохождений теста: {{ props.messageEl.resReg }}</div>
			<div>Количество попыток прохождений теста: {{ props.messageEl.attempts }}</div>
		</div>
		<button @click="deleting">Удалить пользователя</button>
	</div>
</template>




<style scoped>
.userCard {
	border: 1px solid #ccc;
	padding: 10px;
	margin: 5px 0;
	border-radius: 5px;
}

.cards {
	display: grid;
	grid-template-columns: repeat(5, 1fr);
	gap: 10px;
}

.active-card {
	background-color: blueviolet;
	border: solid 1px pink;
}
</style>
