<template>
    <div class="tennis-coaching">
        <h1>{{ pageTitle }}</h1>
        <div class="coach-info">
            <!-- 1a. Text Interpolation - the coachname is given the value aditya pratap -->
            <p>Welcome to {{ coachName }}'s Tennis Coaching!</p>

            <!-- 1b. Raw HTML [v-html], v-html on the <p> element binds it to the htmlContent data property. -->
            <p>{{ coachingDescription }}</p>
        </div>

        <!-- 1c. Attribute Bindings [v-bind:id], 2. Method being called on click -->
        <!-- 5a. Binding HTML class to conditionally apply the "active" class to the button -->
        <!-- 5b. Binding Inline Styles to conditionally change the background color-->
        <!-- 7b. Event Handling: Listening to Events :Inline Handlers -->
        <button :id="scheduleButtonId" class="schedule-button" @click="appointmentVisibility"
            :class="{ 'active': isAppointmentVisible }"
            :style="{ 'background-color': isAppointmentVisible ? '#ff5733' : '#007bff' }">
            {{ scheduleButtonText }}
        </button>

        <!-- 1d. JavaScript expressions inside syntax i.e.{{ }} -->
        <p class="schedule-time" v-if="isAppointmentVisible">{{ formatScheduleTime() }}</p>

        <!-- 6a. List Rendering with v-for and an Object which is appointment -->
        <div class="appointment-list">
            <h2>Available Appointments</h2>
            <ul>

                <!-- 6c. Using v-for on <template> -->
                <template v-for="(appointment, time) in appointments">

                    <!-- 6d. Using v-for with v-if, not showing the appointment that has already been booked and only showing the available appointments-->
                    <li v-if="time !== '10:00 AM'" :key="appointment">
                        {{ time }}: {{ appointment }}
                    </li>
                </template>
            </ul>
        </div>

        <!-- 6b. List Rendering with v-for and a Range -->
        <div class="number-list">
            <h2>List rendering with a range : </h2>
            <h2> Numbers</h2>
            <ul>
                <li v-for="number in numberRange" :key="number">
                    {{ number }}
                </li>
            </ul>
        </div>

        <!-- 8. Form Input Bindings  a. v-model with <input type="text">, <input type="checkbox">, <input type="radio">, <select> and <textarea> -->
        <!-- b. v-model modifiers [.lazy , .number, .trim] -->
        <!-- Form to Schedule Appointment -->
        <form @submit.prevent="scheduleAppointment">
            <h2>Schedule an Appointment</h2>

            <!-- Text Input for Name -->
            <div>
                <label for="name">Your Name:</label>
                <input type="text" id="name" v-model.trim="name">
            </div>

            <!-- Checkbox for Online/Offline Session -->
            <div>
                <label for="sessionType">Session Type:</label>
                <input type="checkbox" id="sessionType" v-model="onlineSession">
                <label for="sessionType">Online</label>
            </div>

            <!-- Radio Buttons for Age Group -->
            <div>
                <label for="ageGroup">Age Group:</label>
                <input type="radio" id="ageGroupAbove18" value="above18" v-model="ageGroup">
                <label for="ageGroupAbove18">Above 18</label>
                <input type="radio" id="ageGroupBelow18" value="below18" v-model="ageGroup">
                <label for="ageGroupBelow18">Below 18</label>
            </div>

            <!-- Select Dropdown for Appointment Time -->
            <div>
                <label for="appointmentTime">Select a time:</label>
                <select v-model="selectedAppointmentTime" id="appointmentTime">
                    <option v-for="(appointment, time) in appointments" :value="time" :key="time">
                        {{ time }}
                    </option>
                </select>
            </div>

            <button type="submit">Schedule</button>
        </form>


        <!-- 11. Router links -->
        <div class="navigation-container">
            <h3>Adding routes</h3>

            <router-link to="/contact">Contact Us</router-link>
            <router-link to="/about">About</router-link>
            <router-view></router-view>

        </div>


    </div>
</template>
  
<script setup>
import { ref, computed } from 'vue';


const pageTitle = 'Tennis Coaching';
const coachName = 'Aditya Pratap';
const coachingDescription = 'Learn to play tennis like a pro!';
const scheduleButtonId = 'schedule-button';
const scheduleTime = '10:00 AM';

// 3. isAppointmentVisible is a reactive property, <script setup> has been used to define component logic
const isAppointmentVisible = ref(false);

// 4. This is a computed Property that calculates its value based on the isAppointmentVisible data property. 
const scheduleButtonText = computed(() => {
    return isAppointmentVisible.value ? 'Hide Appointment' : 'Show Appointment';
});

function formatScheduleTime() {
    return `Next session at ${scheduleTime}`;
}


// 7b. Event Handling: Listening to Events :Method Handler
function appointmentVisibility() {
    isAppointmentVisible.value = !isAppointmentVisible.value;
}

const name = ref('');
const onlineSession = ref(false);
const ageGroup = ref('above18');
const selectedAppointmentTime = ref('');

// 6a. List Rendering with an Object
const appointments = {
    '9:00 AM': 'Session 1',
    '10:00 AM': 'Session 2',
    '11:00 AM': 'Session 3',
    '2:00 PM': 'Session 4',
};

// 6b. List Rendering with a Range
const numberRange = ref(Array.from({ length: 5 }, (_, index) => index + 1));


// Method to Handle Form Submission, logging it to the console for now
function scheduleAppointment() {
    console.log('Name:', name.value);
    console.log('Online Session:', onlineSession.value);
    console.log('Age Group:', ageGroup.value);
    console.log('Selected Appointment Time:', selectedAppointmentTime.value);
}


</script>
  
<style scoped>
.tennis-coaching {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100%;
    width: 90vw;
    margin: 0;
    padding: 0;
    font-family: Arial, sans-serif;
    background-color: #7b8a9e;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
    box-sizing: border-box;
}

h1 {
    font-size: 36px;
    color: #d3dee0;
    margin-top: 0;
}

.coach-info {
    margin: 20px 0;
}

p {
    font-size: 18px;
    color: #010720;
    margin: 10px 0;
}

.schedule-button {
    font-size: 20px;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.schedule-button:hover {
    background-color: #0056b3;
}

.schedule-time {
    font-size: 16px;
    color: #110c0c;
}

/* 5a. Binding HTML class */
.active {
    background-color: #ff5733;
}

.appointment-list {
    margin-top: 20px;
}

.appointment-list h2 {
    font-size: 24px;
    color: #010720;
}

.appointment-list ul {
    list-style: none;
    padding: 0;
    margin: 0;
}

.appointment-list li {
    font-size: 18px;
    color: #110c0c;
    margin: 5px 0;
}

.number-list {
    margin-top: 20px;
    display: flex;
    color: black;
    padding-top: 2rem;
}

.number-list h2 {
    font-size: 24px;
    color: #010720;
}

.number-list ul {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    flex-wrap: wrap;
}

.number-list li {
    font-size: 18px;
    color: #110c0c;
    margin: 5px;
}

form {
    margin-top: 20px;
    width: 100%;
    max-width: 400px;
    background-color: #fff;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
    box-sizing: border-box;
}


.navigation-container {
    margin-top: 20px;
    width: 100%;
    max-width: 400px;
    background-color: #fff;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
    box-sizing: border-box;
}
</style>
  
  
  