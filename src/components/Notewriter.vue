<template>
    <main>

        <header class="header bg-color-primary">
            <div class="row">
                <div class="columns large-12">
                    <h6 class="color-white">SCP demo</h6>
                </div>
            </div>
        </header>

        <section class="notewriter">
            <div class="row">
                <div class="large-2 columns">

                    <div class="notewriter-actions">
                        <div class="button-group">
                            <button class="button" v-on:click="addNewPatient">Add patient</button>
                            <div class="dropdown">
                                <button class="button has-dropdown-icon">Other</button>
                                <div class="dropdown-body">
                                    <a class="dropdown-item" href="#">Some action</a>
                                    <a class="dropdown-item" href="#">Another action</a>
                                    <span class="dropdown-item-sep"></span>
                                    <a class="dropdown-item" href="#">CSS dropdown without JS</a>
                                </div>
                            </div>
                        </div>
                    </div>

                    <List v-bind:patient="patient" v-bind:activePatientIndex="activePatientIndex" />

                </div>
                <div class="large-8 columns">

                    <div class="_ck-editors">
                        <div class="_ck-editor" v-for="(p, index) in patient" v-bind:key="p.id" v-bind:class="{ active: activePatientIndex == index }">
                            <Editor v-bind:patient="p" />
                        </div>
                    </div>


                    <pre><code>{{ patient }}</code></pre>

                </div>
                <div class="large-2 columns">
                    <p>Other widgets</p>
                </div>
            </div>
        </section>

    </main>
</template>

<script>
    
    import moment from 'moment'
    import List from './List.vue'
    import Editor from './Editor.vue'
    import { EventBus } from './../EventBus'
    import _ from 'lodash'

    export default {
        name: 'Notewriter',

        components: {
            List,
            Editor
        },

        data() {
            return {
                message: 'this is test message!',
                editors: {},
                patient: [
                    { name: 'Test name', createdAt: moment().format('MMMM Do YYYY, h:mm:ss a'), id: this.generateUniqueId(), notes: '' },
                    { name: 'Another name', createdAt: moment().format('MMMM Do YYYY, h:mm:ss a'), id: this.generateUniqueId(), notes: '' }
                ],
                activePatientIndex: 0
            }
        },

        created() {
            EventBus.$on('removePatient', id => {
                this.patient =  _.remove(this.patient, (p) => {
                    return p.id !== id;
                });

                this.activePatientIndex = this.patient.length - 1;
            });

            EventBus.$on('selectPatient', index => this.activePatientIndex = index);
        },

        mounted() {

        },

        methods: {
            generateUniqueId() {
                return moment().unix() + '.' + Math.random().toString(36).substr(2, 9);
            },

            addNewPatient() {
                this.patient.push({ name: 'New patient', createdAt: moment().format('MMMM Do YYYY, h:mm:ss a'), id: this.generateUniqueId(), notes: '' });
                this.activePatientIndex = this.patient.length - 1;
            }
        },

        computed: {
            
        },

        watch: {
            // patient: {
            //     handler(value, oldValue) {
            //         let storage = JSON.parse(JSON.stringify(value));

            //         console.log(storage);
            //     },
            //     deep: true
            // }
        }
    }
</script>

<style>
.header {
    padding: 15px 0;
}

.notewriter {
    padding: 20px 0;
}

.dropdown-body {
    top: 100%;
}

.notewriter-plist>a {
    display: block;
    margin: 10px 0;
    opacity: 0.5;
}

.notewriter-plist>a.active {
    opacity: 1;
}

.form-inline {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 20px;
}

._ck-editors ._ck-editor {
    display: none;
}

._ck-editors ._ck-editor.active {
    display: block;
}
</style>


