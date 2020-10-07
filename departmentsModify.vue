<template>
    <div id="modify-dept">
        <b-container>
                <b-row>
                    <b-col cols="12" lg="12">
                        <p class="h2 text-left">{{ edit ? 'Edit' : 'Create' }} Department</p>
                    </b-col>
                </b-row>
                <b-row>
                    <b-col cols="12" lg="6">
                        <b-row class="pb-3">
                            <b-col>
                                <b-form-group
                                    id="name-group"
                                    description="Name of department"
                                    label="Name:* "
                                    label-for="input-name"
                                    label-cols-sm="4"
                                    label-cols-lg="3"
                                    :invalid-feedback="'Please enter a name'"
                                    :state="$data.states.nameState"
                                >
                                    <b-form-input
                                        id="input-name"
                                        v-model="$data.form.name"
                                        :state="$data.states.nameState"
                                        @change="$data.states.nameState = ($data.form.name.length > 0)"
                                    ></b-form-input>
                                </b-form-group>
                            </b-col>
                        </b-row>
                        <b-row class="pb-3">
                            <b-col>
                                <b-form-group
                                    id="abbreviation-group"
                                    description="Abbreviation of department name"
                                    label="Abbreviation: "
                                    label-for="input-abbreviation"
                                    label-cols-sm="4"
                                    label-cols-lg="3"
                                >
                                    <b-form-input
                                        id="input-abbreviation"
                                        v-model="form.abbreviation"
                                    ></b-form-input>
                                </b-form-group>
                            </b-col>
                        </b-row>
                        <b-row class="pb-5">
                            <b-col>
                                <b-form-group
                                    id="description-group"
                                    description="Description of department"
                                    label="Description:* "
                                    lable-for="input-description"
                                    label-cols-sm="4"
                                    label-cols-lg="3"
                                    :invalid-feedback="'Please enter a description'"
                                    :state="$data.states.descriptionState"
                                >
                                    <b-form-textarea
                                        id="input-description"
                                        label="Department Description"
                                        v-model="form.description"
                                        :state="$data.states.descriptionState"
                                        @change="$data.states.descriptionState = (form.description.length > 0)"
                                    ></b-form-textarea>
                                </b-form-group>
                            </b-col>
                        </b-row>
                    </b-col>
                    <b-col cols="12" lg="6">
                        <b-row>
                            <b-col cols="12" lg="6">
                                <p>Select Department Parent:</p>
                            </b-col>
                            <b-col cols="12" lg="6">
                                <treeselect
                                    id="departments"
                                    class="mb-5"
                                    v-model="form.parent"
                                    placeholder="Parent Department"
                                    :multiple="false"
                                    :options="$store.state.departments.departments"
                                    :show-count="true"
                                    :default-expand-level="1"
                                    :flat="true"
                                    :always-open="false"
                                ></treeselect>
                            </b-col>
                        </b-row>
                        <b-row class="pb-3">
                            <b-col cols="12" lg="6">
                                <p>Attach Strategic Objectives:</p>
                            </b-col>
                            <b-col cols="12" lg="6">
                                <treeselect
                                    id="strategic_objectives"
                                    class="mb-5"
                                    v-model="form.strategic_objectives"
                                    placeholder="Strategic Objectives"
                                    :multiple="true"
                                    :load-options="(node) => { $store.dispatch('strategicObjectives/getStrategicObjectives'); node.callback(); }"
                                    :options="$store.state.strategicObjectives.strategicObjectives"
                                    :show-count="true"
                                    :default-expand-level="1"
                                    :flat="true"
                                    :always-open="false"
                                ></treeselect>
                            </b-col>
                        </b-row>
                    </b-col>
                </b-row>
                <submit-reset-buttons :submitFunction="onSubmit" :resetFunction="onReset">
                </submit-reset-buttons>
        </b-container>
    </div>
</template>

<script>
import Treeselect from '@riophae/vue-treeselect'
import '@riophae/vue-treeselect/dist/vue-treeselect.css'
import SubmitResetButtons from '../../components/util/SubmitResetButtons.vue'

export default {
	name: 'AddDepartment',
	components: {
        Treeselect, SubmitResetButtons
    },
	props: {
        departmentId: {
            required: true,
        },
		edit: {
            required: true,
            type: Boolean,
        }
	},
	data()  {
		return  {
            department: null,
            states: {
                nameState: null,
                descriptionState: null,
            },
            emptyForm: {
                name: null,
                abbreviation: null,
                description: null,
                parent: null,
                strategic_objectives: null,
            },
			form:   {
				name: '',
				abbreviation: '',
				description: '',
				parent: null,
				strategic_objectives: [],
			},
		}
	},
    mounted() {
        if (this.$props.edit) {
            let finder = (arr) => {
                for (let i = 0; i < arr.length; i++) {
                    console.log(this.$route.params.departmentId, arr[i], arr, this.$route)
                    if (this.$route.params.departmentId == arr[i].id)
                    {
                        this.$data.form.id = arr[i].id;
                        return arr[i];
                    } else {
                        let possibleDept = finder(arr[i].children);
                        if (possibleDept != undefined)
                            return possibleDept;
                    }
                }
                return undefined;
            };
            this.$data.department = finder(this.$store.state.departments.departments || []);
            if(this.$data.department === undefined) {
                this.$store.dispatch('departments/getDepartments', { callback: (data) => {
                    this.$data.department = finder(data);
                    if (this.$data.department == null)
                        this.$router.push({ name: '404' });
                    this.onReset();
                }});
            }
        }
    },
    methods: {
		onSubmit()   {
            if(this.$props.edit) {
                this.$data.form.isComplete = 0;
                this.$data.form.added_by = 1;
                this.$data.form.modified_by = 1;
                this.$store.dispatch('departments/updateDepartment', { objToSave: this.$data.form });
            } else {
                /* change this after users added */
                this.$data.form.added_by = 1;
                this.$data.form.modified_by = 1;
                this.$data.form.added_by_date = this.$data.today;
                this.$store.dispatch('departments/saveDepartment', { objToSave: this.$data.form });
            }
        },
		onReset() {
            if (this.$props.edit) {
                Object.keys(this.$data.form).forEach((key) => {
                    this.$data.form[key] = this.$data.department[key];
                });
                Object.keys(this.$data.states).forEach((key) => {
                    this.$data.states[key] = true;
                });
            } else {
                Object.keys(this.$data.form).forEach((key) => {
                    this.$data.form[key] = this.$data.emptyForm[key];
                });
                Object.keys(this.$data.states).forEach((key) => {
                    this.$data.states[key] = null;
                });
            }
        }
	}
}
</script>

<style scoped>

</style>	