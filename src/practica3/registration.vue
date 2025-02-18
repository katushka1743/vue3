<script setup>
import { ref, reactive } from 'vue'
import modal from '../modal/modal.vue';

const local = reactive({
	name: '', // переменная для записи имени из input
	gender: '', // переменная для записи пола из input
	age: '', // переменная для записи возраста из input
	userData: {
		name: '', // переменная для присовения имени 
		gender: '', // переменная для присовения пола 
		age: '', // переменная для присовения возраста 
		resReg: 0, // переменная для записи результата теста
		attempts: 0, // переменная для записи попыток прохождений теста
	},
	errorName: false, // переменная для вывода ошибок в input 
	errorGender: false, // переменная для вывода ошибок в input 
	errorAge: false, // переменная для вывода ошибок в input 
	regName: /^[А-Я][а-я]+$/, // регулярное выражения для проверки имени
	errorOut: 'Данное поле заполнено неверно или не заполнено', // Текст для вывод ошибки в input
})

// Функция для проверки ввода в input. Если имя, пол и возраст пустые, то ошибка. В имени иначе - переменная с ошибкой принимает true (показывается), если регулярное выражение не прошло проверку и false (исчезает), если прошло. В в озрасте проверка, что введённое число больше 5 и меньше 100. Если все поля ошибок скрыты (False), то в объект userData вносятся данные. Они отправляюся в emit. Поля ввода очищаются
function checkInput() {
	if (local.name == '') {
		local.errorName = true
	} else {
		local.errorName = !local.regName.test(local.name);
	}

	if (local.gender == '') {
		local.errorGender = true
	} else {
		local.errorGender = false
	}

	if (local.age == '') {
		local.errorAge = true
	} else {
		if (5 <= Number(local.age) && Number(local.age) <= 100) {
			local.errorAge = false
		} else {
			local.errorAge = true
		}
	}

	if (local.errorName == false && local.errorGender == false && local.errorAge == false) {
		local.userData = {
			name : local.name,
			gender: local.gender,
			age: local.age,
			resReg: 0,	
			attempts: 0,
		}

		emit('usersEmit', local.userData)

		local.name = ''
		local.gender = ''
		local.age = ""
	}
}

// Отправка объекта userData с данными
const emit = defineEmits ([
    'usersEmit',
])
</script>


<template>
	<h1>Создание пользователя</h1>
	<h3>Введите имя на русском языке с большой буквы</h3>
	<input type="text" name="name" id="name" placeholder="Имя" v-model="local.name" class="inp">
	<div v-if="local.errorName" class="error">{{local.errorOut}}</div>

	<h3>Выберите пол</h3>
	<input type="radio" id="male" value="Мужской" v-model="local.gender">
	<label for="male">Мужской</label>

	<input type="radio" id="female" value="Женский" v-model="local.gender">
	<label for="female">Женский</label>
	
	<input type="radio" id="what" value="Нечто" v-model="local.gender">
	<label for="what">Нечто/Ничто</label>
	<div v-if="local.errorGender" class="error">{{local.errorOut}}</div>

	<h3>Введите возраст числом от 5 до 100</h3>
	<input type="number" name="age" id="age" placeholder="Возраст" v-model="local.age" min="5" max="100" class="inp">
	<div v-if="local.errorAge" class="error">{{local.errorOut}}</div>

	<br><br>
	<button @click="checkInput">Подтвердить</button>
</template>



<style scoped>
.error {
	color: red;
}

.inp {
	width: 150px;
	height: 30px;
}
</style>
