<template>
    <div>
        <div>
            <b-button variant="info" @click="submitFunction" class="mr-3 float-right">Submit</b-button>
        </div>
        <div>
            <b-button variant="outline-secondary" @click="resetFunction" class=" float-right">Reset</b-button>
        </div>

        <!--https://www.lukew.com/ff/entry.asp?571 Primary & Secondary Actions in Web Forms-->
    </div>
</template>

<script>
    export default {
        name: "SubmitResetButtons",
        props: {
            submitFunction: {
                type: Function,
                required: true,
            },
            resetFunction: {
                type: Function,
                required: true,
            }
        }
    }
</script>
