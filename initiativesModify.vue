<template>
	<div id="add-init">
		<b-container>
			<!-- Top portion of component -->
			<b-row class="pb-3">
				<!-- Left portion of component -->
				<b-col cols="12" lg="6" class="px-3 pb-3">
					<!-- Initiative Name -->
					<b-row>
						<b-col>
							<b-form-group
								id="name-field"
								description="Name of Initiative"
								label="Initiative"
								label-for="init-name"
								label-cols-sm="4"
								label-cols-lg="3"
								:invalid-feedback="'Please enter a name.'"
								:state="states.nameState"
							>
								<b-form-input
									id="init-name"
									v-model="form.name"
									:state="states.nameState"
									@change="states.nameState = (form.name.length > 0)"
								></b-form-input>
							</b-form-group>
						</b-col>
					</b-row>

					<!-- Strategic Objective and Priority Number -->
					<b-row>
						<b-col>
							<b-form-group
								id="so-field"
								description="Choose a strategic objective as this initiative's parent."
								label="Strategic Objective"
								label-for="strategic-objective"
								label-cols-sm="4"
								label-cols-lg="3"
								:invalid-feedback="'Please choose a strategic objective.'"
								:state="states.soState"
							>
								<b-form-select
									id="strategic-objective"
									v-model="form.strat_obj"
									:options="stratOptions"
									:state="states.soState"
									@change="states.soState = (form.strat_obj != null)"
								></b-form-select>
							</b-form-group>
						</b-col>
					</b-row>

					<!-- Initiative Description -->
					<b-row>
						<b-col>
							<b-form-group
								id="description-field"
								description="Give a description of this initiative."
								label="Initiative Description"
								label-for="description"
								label-cols-sm="4"
								label-cols-lg="3"
								:invalid-feedback="'Please give a description.'"
								:state="states.descState"
							>
								<b-form-input
									id="description"
									v-model="form.description"
									:state="states.descState"
									@change="states.descState = (form.description.length > 0)"
								></b-form-input>
							</b-form-group>
						</b-col>
					</b-row>

					<!-- Date Fields -->
					<b-row>
						<b-col>
							<b-form-group
								id="start-date-field"
								description="Choose a starting date for this initiative."
								label="Start Date"
								label-for="start-date"
								label-cols-sm="4"
								label-cols-lg="3"
								:invalid-feedback="'Please choose a date.'"
								:state="states.startState"
							>
								<b-form-datepicker
									id="start-date"
									v-model="form.start_date"
									:min="now"
									:state="states.startState"
									@hidden="updateDate"
									@input="states.startState = (form.start_date != null)"
								></b-form-datepicker>
							</b-form-group>
						</b-col>
					</b-row>
					<b-row>
						<b-col>
							<b-form-group
								id="end-date-field"
								description="Choose an ending date for this initiative."
								label="End Date"
								label-for="end-date"
								label-cols-sm="4"
								label-cols-lg="3"
								:invalid-feedback="'Please choose a date.'"
								:state="states.endState"
							>
								<b-form-datepicker
									id="end-date"
									v-model="form.end_date"
									:min="form.start_date"
									:state="states.endState"
									@input="states.endState = (form.end_date != null)"
								></b-form-datepicker>
							</b-form-group>
						</b-col>
					</b-row>
				</b-col>

				<!-- Right portion of component -->
				<b-col cols="12" lg="5" offset-lg="1" class="px-3 pb-3">
					<b-row>
						<b-col>
							<b-form-group
								id="priority-field"
								description="(optional) 1 to 3, how pressing is this initiative?"
								label="Priority Number"
								label-for="priority-number"
								label-cols-sm="4"
								label-cols-lg="3"
								:state="states.priorityState"
							>
								<b-form-select
									id="priority-number"
									v-model="form.priority_number"
									:options="priorityOptions"
									:state="states.priorityState"
									@change="states.priorityState = (form.priority_number != null)"
								></b-form-select>
							</b-form-group>
						</b-col>
					</b-row>
					<b-row>
						<b-col>
							<b-form-group
								id="user-field"
								description="Select user(s) responsible for initiative."
								label="Users"
								label-for="user-select"
								label-cols-sm="4"
								label-cols-lg="3"
								:invalid-feedback="'Please select one or more users.'"
								:state="states.userState"
							>
								<treeselect
									id="user-select"
									class="treeselect-is-invalid"
									:multiple="true" 
									:load-options="(node) => { $store.dispatch('users/getUsers'); node.callback(); }"
									:auto-load-root-options="Boolean(1)"
									:options="$store.state.users.users"
									:state="states.userState"
									:normalizer="userNormalizer"
									@input="states.userState = userValidation(false)"
									placeholder=""
									v-model="form.users"
								></treeselect>
							</b-form-group>
						</b-col>
					</b-row>
					<b-row>
						<b-col>
							<b-form-group
								id="tag-field"
								description="Select tag(s) for this initiative."
								label="Tags"
								label-for="tags"
								label-cols-sm="4"
								label-cols-lg="3"
								:invalid-feedback="'Please give this initiative a tag.'"
								:state="states.tagState"
							>
								<b-form-input
									id="tags"
									v-model="form.tags"
									:state="states.tagState"
									@change="states.tagState = (form.tags.length > 0)"
								></b-form-input>
							</b-form-group>
						</b-col>
					</b-row>
					<b-row>
						<b-col>
							<b-form-group
								id="target-field"
								description="Select a target as a goal for this initiative."
								label="Target"
								label-for="target"
								label-cols-sm="4"
								label-cols-lg="3"
								:invalid-feedback="'Please give this initiative a target to reach.'"
								:state="states.targetState"
							>
								<b-form-input
									id="target"
									type="number"
									min="1"
									v-model="form.target"
									:state="states.targetState"
									@change="states.targetState = (form.target > 0)"
								></b-form-input>
							</b-form-group>
						</b-col>
					</b-row>
					<b-row>
						<b-col>
							<b-form-group
								id="unit-field"
								description="Select a unit to measure this initiative."
								label="Unit"
								label-for="unit"
								label-cols-sm="4"
								label-cols-lg="3"
								:invalid-feedback="'Please give this initiative a unit to track progress.'"
								:state="states.unitState"
							>
								<b-form-input
									id="unit"
									v-model="form.unit"
									:state="states.unitState"
									@change="states.unitState = (form.unit.length > 0)"
								></b-form-input>
							</b-form-group>
						</b-col>
					</b-row>										
				</b-col>
			</b-row>

			<!-- Save and Cancel -->
			<div>
				<b-button variant="primary" @click="onSubmit()" class="float-right">Submit</b-button>
				<b-button variant="outline-secondary" @click="reset()" class="mr-2 float-right">Reset</b-button>
			</div>
		</b-container>
	</div>
</template>



<script>
import Treeselect from '@riophae/vue-treeselect'
import '@riophae/vue-treeselect/dist/vue-treeselect.css'

export default {
	name: 'AddInitiative',
	components: { Treeselect },
	props: {
		edit: {
			required: true,
			type: Boolean,
		}
	},
	data() {
		const now = new Date();
		const today = now.getFullYear() + "-" + now.getMonth() + "-" + now.getDate();
		return {
			initiative: null,
			resetVal: 1,
			now: now,
			today: today,
			end_date_disabled: true,
			states: {
				nameState: null,
				soState: null,
				descState: null,
				startState: null,
				endState: null,
				priorityState: null, //optional
				userState: null,
				tagState: null,
				unitState: null,
				targetState: true,
			},
			emptyForm: {
				strat_obj: null,
				name: null,
				description: null,
				priority_number: null,
				unit: null,
				target: 100,
				isComplete: 0,
				added_by: null,
				modified_by: null,
				added_by_date: null,
				modified_by_date: null,
				start_date: null,
				end_date: null,
				label: null,
				text: null,
				tags: null,
				users: null,
			},
			form: {
				strat_obj: null,
				name: null,
				description: null,
				priority_number: null,
				unit: null,
				target: 100,
				isComplete: 0,
				added_by: null,
				modified_by: null,
				added_by_date: null,
				modified_by_date: null,
				start_date: null,
				end_date: null,
				label: null,
				text: null,
				tags: null,
				users: null,
			},
			stratOptions: [],
			userOptions: [],
			priorityOptions: [
				{ value: "1", text: "1" },
				{ value: "2", text: "2" },
				{ value: "3", text: "3" },
			],
		}
	},
	mounted() {
		this.fillStrategicObjectives();

		if (this.$props.edit) {
			let finder = (arr) => arr.find((init) => {
				this.$data.form.id = init.id;
				return this.$route.params.initiativeId == init.id; });
			this.$data.initiative = finder(this.$store.state.initiatives.initiatives || []);
			if(this.$data.initiative === undefined) {
				this.$store.dispatch('initiatives/getInitiatives', { callback: (data) => {
					this.$data.initiative = finder(data);
					if (this.$data.initiative == null)
						this.$router.push({ name: '404' });
				}});
			}
			this.reset();
		}
	},
	methods: {
		fillStrategicObjectives() {
			this.$store.dispatch('strategicObjectives/getStrategicObjectives', {callback: (stratObjList) => {
				stratObjList.forEach( (sObj) => {
					this.$data.stratOptions.push({value: sObj.id, text: sObj.name});
				});
			}});
		},
		userNormalizer(node) {
			node.label = node.first_name + ' ' + node.last_name;
			node.children = null;
		},
		updateDate() {
			if (this.$data.form.end_date != null && this.$data.form.start_date >= this.$data.form.end_date) {
				this.$data.form.end_date = null;
				this.$data.states.endState = false;
			}
		},
		userValidation() {
			if (!this.$data.resetVal) {
				this.$data.resetVal++;
				return null;
			} else {
				return !(this.$data.form.users == null || !this.$data.form.users.length);
			}
		},
		reset() {
			if (this.$props.edit) {
				Object.keys(this.$data.form).forEach((key) => {
					this.$data.form[key] = this.$data.initiative[key];
				});
				Object.keys(this.$data.states).forEach((key) => {
					this.$data.states[key] = true;
				});
			} else {
				this.$data.resetVal = 0;
				Object.keys(this.$data.form).forEach((key) => {
					this.$data.form[key] = this.$data.emptyForm[key];
				});
				Object.keys(this.$data.states).forEach((key) => {
					this.$data.states[key] = null;
				});
				//find a way to set users and tags
			}
		},
		onSubmit() {
			//NOTE: NEED A WAY TO SEND USERS AND TAGS
			if (this.$data.states.nameState && this.$data.states.soState && this.$data.states.descState
			&& this.$data.states.startState && this.$data.states.endState && this.$data.states.userState
			&& this.$data.states.tagState && this.$data.states.unitState && this.$data.states.targetState) {
				console.log(this.$data.form)
				//NOTE: the following section is temporary so that the post request has the information it needs
				this.$data.form.modified_by = 1;
				//END TEMPORARY SECTION
				this.$data.form.label = this.$data.form.name;
				this.$data.form.text = this.$data.form.name;
				this.$data.form.modified_by_date = this.$data.today;

				if (this.$props.edit) {
					this.$data.form.isComplete = 0;
					this.$store.dispatch('initiatives/updateInitiative', { objToSave: this.$data.form });
				} else {
					this.$data.form.added_by = 1;
					this.$data.form.added_by_date = this.$data.today;
					this.$store.dispatch('initiatives/saveInitiative', { objToSave: this.$data.form });
				}
			} else {
				if (this.$data.nameState == null)
					this.$data.nameState = false;
				if (this.$data.soState == null)
					this.$data.soState = false;
				if (this.$data.descState == null)
					this.$data.descState = false;
				if (this.$data.startState == null)
					this.$data.startState = false;
				if (this.$data.endState == null)
					this.$data.endState = false;
				if (this.$data.userState == null)
					this.$data.userState = false;
				if (this.$data.tagState == null)
					this.$data.tagState = false;
				if (this.$data.unitState == null)
					this.$data.unitState = false;
				if (this.$data.targetState == null)
					this.$data.targetState = false;
			}
		}
	}
}
</script>



<style scoped>
</style>