<template>
    <b-container class="hello" fluid>


        <!----------------------------------------------------------------------------->

        <b-row class="mt-5">

            <b-col class="ml-4" sm="2">

                <!-- <div class="headerSettings">
                    // <h2>Departments</h2>
                 </div> -->

                <async-tree
                        id="departments2"
                        class="mb-5"
                        placeholder="Departments"
                        :options-vuex="$store.state.departments.departments"
                        :set-selections="$store.state.departments.selected"
                        :load-options-vuex="() => { $store.dispatch('departments/getDepartments') }"
                        :update-selections-vuex="(items) => { $store.commit('departments/setSelected', items) }"
                        @selected-changed="onDepartmentsSelectionChange"

                ></async-tree>


            </b-col>
            <!--
                        <b-col cols="8" class="mb-2" style="">
                            <vue-custom-scrollbar class="scroll-area" :settings="settings" @ps-scroll-y="scrollHanle">
                                <p>Departments Selected: {{ this.$store.state.departments.selected }}</p>
                                <div v-for="selected in $store.state.departments.selected" v-bind:key="selected">
                                    <div v-for="j in $store.state.departments.departments" v-bind:key="j.id">
                                        <div v-if="j !== null && j.id === selected">
                                            <p>id: {{j.id}} / Name: {{j.name}} / Description: {{j.description}} / lft:
                                                {{j.lft}} /
                                                rght: {{j.rght}}</p>
                                        </div>
                                    </div>
                                </div>
                            </vue-custom-scrollbar>

                        </b-col>
            -->

        </b-row>
        <!------------------------------------------------------------------------------------------------------------------->
        <b-row>
            <b-col cols="12" class="mt-2 mb-5 ml-0" sm="5" style="background: lightgrey; height:67vh;">
                <b-row style="background-color: dimgray;">
                    <b-col class="text-left mt-1" style="background-color: dimgray; color:white;"><h2>Strategies</h2>
                    </b-col>
                </b-row>
                <b-row>
                    <b-col >
                        <h5 class="mt-0 pt-2 text-left"><u>Academic Excellence</u></h5>
                        <p class="text-left">Through our commitment to discovery in all its forms, we
                            support a culture of scholarship in which the contribution of
                            each discipline is celebrated. The breadth and recognition of
                            our curriculum and academic programs reflect the highest standards
                            of rigor, quality, and delivery
                        </p>
                    </b-col>
                </b-row>

                <b-row >
                    <b-col class="mb-5" sm="5" >
                        <async-tree
                                id="strategicObjectives2"
                                class="ml-3 mb-5 pb-5 h-auto"
                                :options-vuex="$store.state.strategicObjectives.strategicObjectives"
                                :set-selections="$store.state.strategicObjectives.selected"
                                :load-options-vuex="getStrategicObjectives"
                                :update-selections-vuex="(items) => { $store.commit('strategicObjectives/setSelected', items) }"
                                @selected-changed="onStrategicObjectivesSelectionChange"

                        ></async-tree>
                    </b-col>
                </b-row>


            </b-col>

            <!-------------------------------------------------------------------------------------------------------------------->

            <b-col cols="12"  sm="7 ml-9" class="mr-0 pr-0">

                <b-col class="mt-2" style="background: lightgrey; height:30vh;">
                    <b-row style="background-color: dimgray;">
                        <b-col class="text-left mt-1" style="background-color: dimgray; color:white;">
                            <h2>Strategy Overview</h2>
                        </b-col>
                    </b-row>
                    <b-row  class="ml-2 mt-5">

                        <b-col  class="text-left pr-0 mr-0"  style="height:35vh;">
                            <vue-custom-scrollbar class="scroll-area-Overview" :settings="settings"
                                                  @ps-scroll-y="scrollHanle">
                                <div v-for="selected in $store.state.strategicObjectives.selected"
                                     v-bind:key="selected">
                                    <div v-for="j in $store.state.strategicObjectives.strategicObjectives"
                                         v-bind:key="j.id">
                                        <div v-if="j !== null && j.id === selected">
                                            <h2>{{j.name}}</h2>
                                            <!--<p><b>id: {{j.id}} / name: {{j.name}} / lft: {{j.lft}} / rght:
                                                {{j.rght}}</b>
                                            </p>-->
                                            <p> {{j.description}}</p>
                                            <br/>
                                        </div>
                                    </div>
                                </div>
                            </vue-custom-scrollbar>
                        </b-col>
                    </b-row>
                </b-col>

                <!---------------------------------------------------------------------->
                <b-col class="mt-5" style="background: lightgrey;">
                    <b-row style="background-color: dimgray;">
                        <b-col class="text-left mt-1 mb-2" style="background-color: dimgray; color:white;">
                            <h2>Strategy Initiatives</h2>
                        </b-col>
                    </b-row>
                    <b-row class="mb-5" style="height:300px;">
                        <b-col  class="text-left mt-4"  style="height:350px;">
                            <async-tree
                                    id="initiatives2"
                                    class=" mt-2"
                                    :options-vuex="$store.state.initiatives.initiatives"
                                    :set-selections="$store.state.initiatives.selected"
                                    :load-options-vuex="getInitiativeOptions"
                                    :update-selections-vuex="(items) => { $store.commit('initiatives/setSelected', items) }"
                            ></async-tree>
                            <p>Initiatives Selected: {{ this.$store.state.initiatives.selected }}</p>

                            <vue-custom-scrollbar class="scroll-area" :settings="settings" @ps-scroll-y="scrollHanle">
                                <div v-for="selected in $store.state.initiatives.selected" v-bind:key="selected">
                                    <div v-for="j in $store.state.initiatives.initiatives" v-bind:key="j.id">
                                        <div v-if="j !== null && j.id === selected">
                                            <p>
                                                <b>id: {{j.id}} / name: {{j.name}}</b>
                                                Description: {{j.description}} <br/>
                                                Target: {{j.target}} / Unit: {{j.unit}} <br/>
                                                Priority Number: {{j.priority_number}} <br/>
                                                isComplete: {{j.isComplete}} <br/>
                                            </p>
                                        </div>
                                    </div>
                                </div>
                            </vue-custom-scrollbar>

                        </b-col>
                    </b-row>
                </b-col>


            </b-col>
        </b-row>



    </b-container>


</template>


<script>
    // import the component
    import AsyncTree from './util/AsyncTree'
    // import the styles
    import '@riophae/vue-treeselect/dist/vue-treeselect.css'
    //import scroll bar
    import vueCustomScrollbar from 'vue-custom-scrollbar'

    export default {
        name: 'Dashboard',
        components: { /*Treeselect,*/ AsyncTree,/*Scroll bar listings */vueCustomScrollbar},
        data: () => {
            return {
                departmentsSelected: [],
                strategicObjectivesSelected: [],
                initiativesSelected: [],
                settings: {
                    maxScrollbarLength: 80
                },

            }
        },
        methods: {
            getStrategicObjectives() {
                if (this.$store.state.departments.selected.length !== 0) {
                    this.$store.dispatch('getStrategicObjectivesFromDepartments', {
                        callback: (response) => {
                            let ids = [];
                            this.gatherIds(response, ids);
                            this.$store.commit('strategicObjectives/setSelected', this.intersect(this.$data.strategicObjectivesSelected, ids))
                        }
                    });
                } else {
                    this.$store.dispatch('strategicObjectives/getStrategicObjectives');
                }
            },
            getInitiativeOptions() {
                if (this.$store.state.strategicObjectives.selected.length !== 0) {
                    this.$store.dispatch('getInitiativesFromStrategicObjectives', {
                        callback: (response) => {
                            let ids = [];
                            this.gatherIds(response, ids);
                            this.$store.commit('initiatives/setSelected', this.intersect(this.$data.initiativesSelected, ids))
                        }
                    });
                } else if (this.$store.state.departments.selected.length !== 0) {
                    this.$store.dispatch('getInitiativesFromDepartments', {
                        callback: (response) => {
                            let ids = [];
                            this.gatherIds(response, ids);
                            this.$store.commit('initiatives/setSelected', this.intersect(this.$data.initiativesSelected, ids))
                        }
                    });
                } else {
                    this.$store.dispatch('initiatives/getInitiatives');
                }
            },
            onDepartmentsSelectionChange() {
                this.$data.strategicObjectivesSelected = this.$store.state.strategicObjectives.selected;
                this.$store.commit('strategicObjectives/setStrategicObjectives', null);
                this.$data.initiativesSelected = this.$store.state.initiatives.selected;
                this.$store.commit('initiatives/setInitiatives', null);
            },
            onStrategicObjectivesSelectionChange() {
                this.$data.initiativesSelected = this.$store.state.initiatives.selected;
                this.$store.commit('initiatives/setInitiatives', null);
            },
            onInitiativesSelectionChange() {
            },
            intersect(arr1, arr2) {
                console.log('intersect before', arr1, arr2);
                arr1 = arr1.filter(x => arr2.includes(x));
                console.log('intersect after', arr1, arr2);
                return arr1
            },
            gatherIds(nodeArr, ids) {
                nodeArr.forEach(item => {
                    if (!('id' in item)) {
                        console.error('given item is missing a id and/or children property!');
                        return;
                    }
                    if (!ids.includes(item.id))
                        ids.push(item.id);
                    if ('children' in item)
                        this.gatherIds(item.children, ids);
                });
            },
            scrollHanle(evt) {
                console.log(evt)

            },
        }
    }
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

    /*
        p {
            margin-left: .5rem;
            margin-right: .5rem;
        }

        h3 {
            margin: 40px 0 0;
        }

        ul {
            list-style-type: none;
            padding: 0;
        }

        li {
            display: inline-block;
            margin: 0 10px;
        }

        a {
            color: #42b983;
        }*/


    .scroll-area {
        position: relative;
        margin: auto;
        width: 90%;
        height: 30%;

    }
    .scroll-area-Overview{
        position: relative;
        margin: auto;
        width: 90%;
        height: 60%;



    }


    /*
        @media screen and (min-width: 601px) {
            .headerSettingsA {
            }
        }

        @media screen and (max-width: 600px) {
            .stackingBlocks {

            }
        }
    */

</style>
