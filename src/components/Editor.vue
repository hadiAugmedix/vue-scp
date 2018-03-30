<template>
    <div>
        <form action="" class="form-inline">
            <div class="form-field">
                <input type="text" name="patient_name" class="form-input" placeholder="e.g Patient name" v-model="patient.name">
            </div>
            <div class="form-field">
                <input type="text" name="patient_time" class="form-input" placeholder="e.g Time" v-model="patient.createdAt">
            </div>
        </form>

        <div class="form-field">
            <textarea name="message" class="form-textarea editor" placeholder="e.g Write notes here!" ref="message" v-model="patient.notes"></textarea>
            <small class="form-text">Lorem ipsum dolor sit amet, consectetur adipisicing elit.</small>
        </div>

        <button class="button" v-on:click="removePatient">Remove patient</button>

        <br><br>
    </div>
</template>

<script>
import ClassicEditor from '@ckeditor/ckeditor5-build-classic'
import { EventBus } from './../EventBus';

export default {
    name: 'Editor',

    props: [
        'patient'
    ],

    data() {
        return {
            editorInstance: null
        }
    },

    mounted() {
        this.createEditor(this.$refs.message, () => {
            this.editorInstance.model.document.on('change', () => {
                this.patient.notes = this.editorInstance.getData();
            });
        });
    },

    methods: {
        createEditor(element, callback) {
            ClassicEditor.create(element).then(et => {
                this.editorInstance = et;

                callback();
            });
        },

        removePatient() {
            EventBus.$emit('removePatient', this.patient.id);
        }
    }
}
</script>

