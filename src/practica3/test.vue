<script setup>
import { ref, reactive } from 'vue'
import modal from '../modal/modal.vue';

const local = reactive ({
    result: 0, // Переменная для записи результата теста
    q1: '',  // Переменная для записи ответа на вопрос 1
    q2: '',  // Переменная для записи ответа на вопрос 2
    q3: '',  // Переменная для записи ответа на вопрос 3
    q4: '',  // Переменная для записи ответа на вопрос 4
    q5: '',  // Переменная для записи ответа на вопрос 5
    q6: '',  // Переменная для записи ответа на вопрос 6
    q7: '',  // Переменная для записи ответа на вопрос 7
    q8: '',  // Переменная для записи ответа на вопрос 8
    q9: '',  // Переменная для записи ответа на вопрос 9
    q10: '', // Переменная для записи ответа на вопрос 10
    modalShow: false, // переменная для показа модального окна
	modalText: '', // переменная для текста модального окна
	modalType: '', // переменная для типа модального окна
})

// отправка emit
const emit = defineEmits([
    'testEmit', 'curPageEmit'
])

// Функция для проерки теста, подсчёта результата.Если пользователь ничего не ответил, то выводится модальное окно с вопросом. Если пользователь не хочет ничего заполнять, то переход в функцию Yes (отправляет результат 0), иначе в функцию No (просто скрывает модальное окно). Иначе происходит проверка всех полей и подсчёт результата. Вывод модального окна с результатом, переход в функцию OK.
function checkTest() {
    if (local.q1 == '' && local.q2 == '' && local.q3 == '' && local.q4 == '' && local.q5 == '' && local.q6 == '' && local.q7 == '' && local.q8 == '' && local.q9 == '' && local.q10 == '') {
        local.modalType = 'answer'
		local.modalText = 'Вы уверены, что не хотите ничего заполнить?'
		local.modalShow = true
    } else {
        if (local.q1 == 'Танос') {
            local.result += 1
        }

        if (Number(local.q2) === 2008) {
            local.result += 1
        }

        if (local.q3 == 'Локи') {
            local.result += 1
        }

        if (Number(local.q4) === 6) {
            local.result += 1
        }

        if (local.q5 == 'Ваканда') {
            local.result += 1
        }

        if (Number(local.q6) === 1) {
            local.result += 1
        }

        if (Number(local.q7) === 1) {
            local.result += 1
        }

        if (local.q8 == 'ЩИТ' || local.q8 == 'Щ.И.Т.' || local.q8 == 'Щ И Т') {
            local.result += 1
        }

        if (Number(local.q9) === 3) {
            local.result += 1
        }


        if (Number(local.q10) === 1) {
            local.result += 1
        }

        local.modalType = 'yes'
		local.modalText = 'Ваш результат: ' + local.result
		local.modalShow = true
    }
}

// Функция для отправки emit результата теста, а также перехода на страницу
function OK() {
    emit('testEmit', local.result)
    emit('curPageEmit')
}

// Функция для отправки emit результата теста 0, а также перехода на страницу
function Yes() {
    local.result = 0
    emit('testEmit', local.result)
    emit('curPageEmit')
}

// Функция для скрытия модального окна
function No() {
    local.modalShow = false
}
</script>



<template>
    <h1>Пройдите тест</h1>
    <div class="questions">
        <div class="question">
            <label>1. Как зовут главного антагониста в фильме "Мстители: Финал"?</label><br>
            <input type="text" name="q1" v-model="local.q1">
        </div>
        
        <div class="question">
            <label>2. В каком году Тони Старк стал Железным человеком в киновселенной Marvel?</label><br>
            <input type="number" name="q2" v-model="local.q2">
        </div>
        
        <div class="question">
            <label>3. Как зовут брата Тора?</label><br>
            <input type="text" name="q3" v-model="local.q3">
        </div>
        
        <div class="question">
            <label>4. Сколько камней бесконечности существует в киновселенной Marvel?</label><br>
            <input type="number" name="q4" v-model="local.q4">
        </div>
        
        <div class="question">
            <label>5. Как называется страна, из которой родом Черная Пантера?</label><br>
            <input type="text" name="q5" v-model="local.q5">
        </div>
        
        <div class="question">
            <label>6. Кто первым подержал Мьёльнир после Тора в "Мстителях: Финал"?</label><br>
            
            <input type="radio" name="q6a" id="q6a" value="1" v-model="local.q6">  
            <label for="q6a">Капитан Америка</label><br>
        
            <input type="radio" name="q6b" id="q6b" value="2" v-model="local.q6">  
            <label for="q6b">Танос</label><br>
        
            <input type="radio" name="q6c" id="q6c" value="3" v-model="local.q6">  
            <label for="q6c">Тони Старк</label><br>
        </div>
        
        <div class="question">
            <label>7. Как зовут супергероя, у которого есть искусственная рука?</label><br>
            <input type="radio" name="q7b" id="q7b" value="2" v-model="local.q7">
            <label for="q7b">Ванда Максимофф</label><br>
            <input type="radio" name="q7a" id="q7a" value="1" v-model="local.q7">
            <label for="q7a">Баки Барнс</label><br>
            <input type="radio" name="q7c" id="q7c" value="3" v-model="local.q7">
            <label for="q7c">Клинт Бартон</label><br>
        </div>
        
        <div class="question">
            <label>8. Как называется организация, которую возглавлял Ник Фьюри?</label><br>
            <input type="text" name="q8" v-model="local.q8">
        </div>
        
        <div class="question">
            <label>9. Сколько фильмов о Человеке-пауке с Томом Холландом вышло в киновселенной Marvel?</label><br>
            <input type="number" name="q9" v-model="local.q9">
        </div>
        
        <div class="question">
            <label>10. Кто спас Тони Старка в начале фильма "Мстители: Финал"?</label><br>
            <input type="radio" name="q10b" id="q10b" value="2" v-model="local.q10">
            <label for="q10b">Доктор Стрэндж</label><br>
            <input type="radio" name="q10c" id="q10c" value="3" v-model="local.q10">
            <label for="q10c">Небула</label><br>
            <input type="radio" name="q10a" id="q10a" value="1" v-model="local.q10">
            <label for="q10a">Капитан Марвел</label><br>
        </div>
    </div>
	<button @click="checkTest">Проверить тест</button>


	<!-- Передача в компонент modal:
		modalType - тип модального окна
		modalText - текст модального окна
	-->
	<!-- Получение из компонента modal emit:
        modalYes и переход в функцию Yes
        modalNo и переход в функцию No
        modalOK и переход в функцию OK
    -->
    <modal
    v-if="local.modalShow"
    :modalType="local.modalType"
    :modalText="local.modalText"
    @modalYes="Yes"
    @modalNo="No"
    @modalOK="OK"
    />
</template>




<style scoped>


</style>
