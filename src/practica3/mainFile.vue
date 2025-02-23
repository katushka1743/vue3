<script setup>
import { ref, reactive } from 'vue'

import registration from './registration.vue';
import outUser from './outUser.vue';
import test from './test.vue';
import modal from '../modal/modal.vue';

const local = reactive({
	curPage: 1, // счётчик страниц
	curUserIndex: null, // счётчик пользователей
	users: [], // массив пользователей
	modalShow: false, // переменная для показа модального окна
	modalText: '', // переменная для текста модального окна
	modalType: '', // переменная для типа модального окна
})

// Функция для перехода на страницу теста. Если пользователь выбран, то переход выполняется. Иначе - вывод модального окна
function moveToTest() {
	if (local.curUserIndex !== null) {
		local.curPage = 3;
	} else {
		local.modalType = 'yes'
		local.modalText = 'Без выбора пользователя вы не можете перейти на страницу теста'
		local.modalShow = true
	}
}

// Функция для проверки при смене пользователя на странице теста. Если пользователь меняется на "нет пользователя", то осуществляется переход на первую страницу, а также выводится модальное окно
function checkAuth() {
	if (local.curPage == 3) {
		if (local.curUserIndex === null) {
			local.curPage = 1;
			local.modalType = 'yes'
			local.modalText = 'Без выбора пользователя вы не можете перейти на страницу теста'
			local.modalShow = true
		} 
	}
}

// Функция для записи из компонента registration данных (имя, пол, возраст, результат выполнения теста и количество попыток прохождения теста) в массив users
function regist(params) {
	local.users.push(params)
	local.curUserIndex += local.users.length - 1
}

//Функция для рассчёта среднего арифмитического прохождений теста. Если пользователь выбран. Для конкретного пользователя, вызванного по индексу, увеличиваем счётчик попыток. Далее записываем пользователю в переменную для результата подсчёт среднего арифметического по каждой его попытке. 
function testMiddle(result) {
    if (local.curUserIndex !== null) {
        local.users[local.curUserIndex].attempts += 1; 
        local.users[local.curUserIndex].resReg = ((local.users[local.curUserIndex].resReg * (local.users[local.curUserIndex].attempts - 1)) + result) / local.users[local.curUserIndex].attempts; 
    }
}

// Функция для удаления пользователя из массива. Если выбран активный пользователь, то пользователь меняется на "нет пользователя". Иначе, если выбран пользователь с индексом больше, чем мы удаляем, сместим индекс на 1 назад. Удаляем из массива пользователя по переданному индексу.
function deleteUser(index) {
    if (local.curUserIndex === index) {
        local.curUserIndex = null; 
    } else if (local.curUserIndex > index) {
        local.curUserIndex = local.curUserIndex - 1; 
    }
    local.users.splice(index, 1);
}
</script>



<template>
	<div class="header">
		<div class="menu">
			<button @click="local.curPage = 1" :class="{active_user : local.curPage == 1}">Зарегистрировать пользователя</button>
			<button @click="local.curPage = 2" :class="{active_user : local.curPage == 2}">Вывод пользователей</button>
			<button @click="moveToTest" :class="{active_user : local.curPage == 3}">Тест</button>
		</div>
		<select v-model="local.curUserIndex" @change="checkAuth">
			<option :value=null>Нет пользователя</option>
			<option :value="index" v-for="(elem, index) in local.users">{{ elem.name }}</option>
		</select>
	</div>

	<!-- Получение из компонента registration emit usersEmit и переход в функцию regist -->
	<registration 
	v-if="local.curPage == 1" 
	@usersEmit="regist"
	/>

	<div v-if="local.curPage == 2">
		<h1>Данные о пользователях</h1> 
		<div class="out">
			<!-- :activeIndex="local.curUserIndex !== null ? local.curUserIndex : -1" означает:
				Если local.curUserIndex !== null, то activeIndex = local.curUserIndex.
				Если local.curUserIndex === null, то activeIndex = -1 
			-->
			<!-- Передача в компонент outUser:
				messageEl - объект с именем, полом, возрастом, результатом теста и попытками
				messageIndex - индекс каждого из пользователей
				mesRes - средний результат теста
				activeIndex - индекст текущего пользователя
			-->
			<!-- Получение из компонента outUser emit deleteUser и переход в функцию deleteUser -->
			<outUser 
			v-for="(data, index) in local.users"
			:messageEl="data"
			:messageIndex="index"
			:mesRes="local.users.resReg"
			:activeIndex="local.curUserIndex !== null ? local.curUserIndex : -1"
			@deleteUser="deleteUser"
			/>
		</div>
	</div>

	<!-- Получение из компонента test:
		emit curPageEmit и изменение счётчика страниц на 2 (outUser)
		emit testEmit и переход в функцию testMiddle
	-->
	<test v-if="local.curPage == 3" 
	@curPageEmit="local.curPage = 2"
	@testEmit="testMiddle"
	/>

	<!-- Передача в компонент modal:
		modalType - тип модального окна
		modalText - текст модального окна
	-->
	<!-- Получение из компонента modal emit modalOK и скрытие модального окна -->
	<modal
    v-if="local.modalShow"
    :modalType="local.modalType"
    :modalText="local.modalText"
    @modalOK="local.modalShow=false"
    />
</template>


<style scoped>
.header {
	display: flex;
	flex-direction: row;
	position: fixed;
	top: 10px;
	left: 10em;
	align-self: center;
	justify-content: center;
	justify-items: center;
	}

.active_user {
	background-color: blueviolet;
	border: solid 1px pink;
}

.out {
	display: grid;
	grid-template-columns: repeat(5, 1fr);
	gap: 10px;
}

.menu {
	min-width: 800px;
	display: flex;
	justify-content: space-around;
}
</style>
