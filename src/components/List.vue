<template>
    <div class="notewriter-plist">
        <a href="" class="avatar" v-for="(p, index) in patient" :key="p.id" v-bind:class="{ active: activePatientIndex == index }" v-on:click="selectPatient($event, index)">
            <figure class="avatar-fig rounded">
                <img src="https://picsum.photos/40/40?image=500" alt="">
            </figure>
            <span class="avatar-label">{{ p.name }}</span>
            <div class="avatar-secondary-block">
                {{ p.createdAt }}
            </div>
        </a>
    </div>
</template>

<script>
import { EventBus } from './../EventBus';

export default {
    name: 'List',

    props: ['patient', 'activePatientIndex'],

    mounted() {
        
    },

    methods: {
        selectPatient($event, index) {
            $event.preventDefault();
            
            EventBus.$emit('selectPatient', index);
        }
    }
}
</script>
