<template>
    <b-container>
        <b-row>
            <b-col cols="12" lg="12">
                <b-row>
                    <b-col>
                        <p class="h1 text-center">Create User</p>
                    </b-col>
                </b-row>

                <b-row class="mt-5">
                    <b-col>
                        <b-row>
                            <b-col lg="6" sm="6">
                                <b-form-group
                                        id="group-first_name"
                                        label="First Name"
                                        label-for="input-first_name"
                                        class="h5 text-center"
                                        label-cols-lg="3"
                                        label-align-lg="right"
                                        :invalid-feedback="invalidFeedback('first_name')"
                                        v-b-tooltip.hover title="Enter your first name"
                                >
                                    <b-form-input
                                            id="input-first_name"
                                            v-model="$v.form.first_name.$model"
                                            :state="validateState('first_name')"
                                            required

                                    >
                                    </b-form-input>
                                </b-form-group>
                            </b-col>

                            <b-col lg="6" sm="6">
                                <b-form-group
                                        id="group-last_name"
                                        label="Last Name"
                                        label-for="input-last_name"
                                        class="h5 text-center"
                                        label-cols-sm="12"
                                        label-cols-lg="4"
                                        label-align-lg="right"
                                        :invalid-feedback="invalidFeedback('last_name')"
                                        v-b-tooltip.hover title="Enter your last name"
                                >
                                    <b-form-input
                                            id="input-last_name"
                                            v-model="$v.form.last_name.$model"
                                            :state="validateState('last_name')"
                                            required
                                    >
                                    </b-form-input>
                                </b-form-group>
                            </b-col>
                        </b-row>

                        <b-row>
                            <b-col lg="12">
                                <b-form-group
                                        id="group-email"
                                        label="Email"
                                        label-for="input-email"
                                        label-cols-lg="1"
                                        class="h5 text-center pl-5"
                                        :invalid-feedback="invalidFeedback('email')"
                                        v-b-tooltip.hover title="Enter your email address"
                                >
                                    <b-form-input
                                            id="input-email"
                                            v-model="$v.form.email.$model"
                                            :state="validateState('email')"
                                            required
                                    >
                                    </b-form-input>

                                </b-form-group>
                            </b-col>
                        </b-row>

                        <b-row>
                            <b-col lg="6">


                                <b-form-group
                                        id="group-password"
                                        label="Password"
                                        label-for="input-password"
                                        class="h5 text-center "
                                        label-cols-sm="12"
                                        label-cols-lg="3"
                                        label-align-lg="right"
                                        :invalid-feedback="invalidFeedback('password')"

                                        v-b-tooltip.hover title="Create a password to associate with your account"
                                >
                                    <b-input-group>
                                        <b-form-input
                                                id="input-password"
                                                v-model="$v.form.password.$model"
                                                :state="validateState('password')"
                                                :type="type"
                                                required

                                        >
                                        </b-form-input>
                                        <b-input-group-append>
                                            <b-button variant="primary" @click="showPassword" pressed>
                                                <BIcon :icon="eyeIcon"></BIcon>
                                                {{pwdBtnTxt}}
                                            </b-button>
                                        </b-input-group-append>
                                    </b-input-group>
                                </b-form-group>


                            </b-col>
                            <b-col lg="6" sm="6">
                                <b-form-group
                                        id="group-confirmPassword"
                                        label="Confirm Password"
                                        label-for="input-confirmPassword"
                                        class="h5 text-center"
                                        label-cols-sm="12"
                                        label-cols-lg="4"
                                        label-align-lg="right"
                                        invalid-feedback="Passwords must match"


                                >
                                    <b-input-group>
                                        <b-form-input
                                                id="input-confirmPassword"
                                                v-model="$v.form.confirmPassword.$model"
                                                :state="validateState('confirmPassword')"
                                                :type="confirmType"
                                                required

                                        >
                                        </b-form-input>
                                        <b-input-group-append>
                                            <b-button variant="primary" @click="showConfirmPassword" pressed>
                                                <BIcon :icon="confirmEyeIcon"></BIcon>
                                                {{confirmBtnTxt}}
                                            </b-button>
                                        </b-input-group-append>
                                    </b-input-group>
                                </b-form-group>

                            </b-col>
                        </b-row>


                        <b-row class="">
                            <b-form-group
                                    id="group-phone_num"
                                    label="Phone Number"
                                    label-for="input-phone_num"
                                    class="h5 align-center"
                                    label-cols="4"
                            >
                                <VuePhoneNumberInput
                                        id="input-phone_num"
                                        v-model="form.phone_num"
                                        valid-color="green"
                                        v-b-tooltip.hover
                                        title="Enter your phone number including your area code"
                                >
                                </VuePhoneNumberInput>
                            </b-form-group>
                        </b-row>

                        <b-row>
                            <b-col lg="12" sm="6">
                                <b-col lg="12" sm="6">

                                    <submit-reset-buttons :submitFunction="onSubmit" :resetFunction="onReset">
                                    </submit-reset-buttons>

                                </b-col>
                            </b-col>
                        </b-row>
                    </b-col>
                </b-row>
            </b-col>
        </b-row>
    </b-container>
</template>

<script>
    import SubmitResetButtons from '../../components/util/SubmitResetButtons.vue'

    import '@riophae/vue-treeselect/dist/vue-treeselect.css'
    import VuePhoneNumberInput from 'vue-phone-number-input';
    import 'vue-phone-number-input/dist/vue-phone-number-input.css';
    import {validationMixin} from "vuelidate";
    import {required, minLength, email, sameAs} from "vuelidate/lib/validators";


    export default {
        name: "AddUser",
        components: {SubmitResetButtons, VuePhoneNumberInput},
        mixins: [validationMixin],
        props: {
            userId: {
                required: true,
            },
            edit: {
                required: true,
                type: Boolean,
            }
        },

        data() {
            return {
                submitStatus: null,

                type: 'password',
                eyeIcon: 'eye',
                pwdBtnTxt: 'Show Password',

                confirmType: 'password',
                confirmEyeIcon: 'eye',
                confirmBtnTxt: 'Show Password',

                emptyForm: {
                    first_name: null,
                    last_name: null,
                    email: null,
                    password: null,
                    confirmPassword: null,
                    phone_num:null,
                },

                form: {
                    first_name: '',
                    last_name: '',
                    email: '',
                    password: '',
                    confirmPassword: '',
                    phone_num: '',
                },

            }
        },
        validations: {
            form: {
                first_name: {
                    required,
                    minLength: minLength(2),

                },
                last_name: {
                    required,
                    minLength: minLength(2),

                },
                email: {
                    required,
                    minLength: minLength(7), //1@1.123  counting for atleast 1 char name, 1 for the @,1 for the host name, 1 for the period, and 3 for the .com
                    email,


                },
                password: {
                    required,
                    minLength: minLength(1),

                },
                confirmPassword: {
                    sameAsPassword: sameAs('password'),

                }
            }
        },

        mounted() {

        },
        methods: {
            validateState(e) {
                const {$dirty, $error} = this.$v.form[e];
                return $dirty ? !$error : null;
            },
            invalidFeedback(e) {
                return (e == 'email') ?
                    ('Enter at least ' + this.$v.form[e].$params.minLength.min + ' characters' + '\n' + 'in the format of example@example.com') :
                    ('Enter at least ' + this.$v.form[e].$params.minLength.min + ' characters')
            },
            showPassword() {
                if (this.type === 'password') {
                    this.type = 'text'
                    this.pwdBtnTxt = 'Hide Password'
                    this.eyeIcon = 'eye-slash'
                } else {
                    this.type = 'password'
                    this.pwdBtnTxt = 'Show Password'
                    this.eyeIcon = 'eye'
                }
            },
            showConfirmPassword() {
                if (this.confirmType === 'password') {
                    this.confirmType = 'text'
                    this.confirmBtnTxt = 'Hide Password'
                    this.confirmEyeIcon = 'eye-slash'
                } else {
                    this.confirmType = 'password'
                    this.confirmBtnTxt = 'Show Password'
                    this.confirmEyeIcon = 'eye'
                }
            },
            onSubmit() {

                if (this.$props.edit) {
                    this.$data.form.isComplete = 0;
                    //this.$data.form.added_by = 1;
                    //this.$data.form.modified_by = 1
                    this.$store.dispatch('users/updateUsers', {objToSave: this.$data.form})

                } else {
                    //this.$data.form.added_by = 1;
                    //this.$data.form.modified_by_date = this.$data.today;
                    //this.$data.form.added_by_date = this.$data.today;
                    this.$store.dispatch('users/saveUser', {objToSave: this.$data.form});
                    console.log(this.$data.form);
                }

            },
            onReset() {
                console.log("1")
                if (this.$props.edit) {
                    console.log("2")
                    Object.keys(this.$data.form).forEach((key) => {
                        this.$data.form[key] = this.$data.user[key];

                    });
                    Object.keys(this.$data.states).forEach((key) => {
                        this.$data.states[key] = true;
                    });
                } else {
                    console.log("3")
                    Object.keys(this.$data.form).forEach((key) => {
                        console.log(key)
                        this.$data.form[key] = this.$data.emptyForm[key];

                    });
                    /*Object.keys(this.$data.states).forEach((key) => {
                        this.$data.states[key] = null;
                    });*/
                }
            }
        }


    }
</script>

<style scoped>

</style>