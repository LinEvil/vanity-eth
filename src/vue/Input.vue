<template>
    <div class="panel">
        <form :class="{error: inputError}">
            <div class="error-text">Numbers and letters from A to F only</div>
            <input type="text" placeholder="Prefix" v-model="prefix" :disabled="running">
            <div class="check">
                <label class="checkbox">
                    <input type="checkbox" name="checkbox" checked="" v-model="checksum"
                           :disabled="running">
                    <i class="left"> </i>
                    Case-sensitive
                </label>
            </div>
            <div class="threads">
                <input type="button" class="square-btn" value="-" @click="threads--"
                       :disabled="running || threads <= 1">
                <input type="button" class="square-btn arrow" value="+" @click="threads++"
                       :disabled="running">
                <h4 v-text="threads"></h4>
                <span>threads</span>
                <span v-if="threads === cores">(recommended)</span>
            </div>
            <div class="row">
                <div class="col-lg-6 col-sm-12">
                    <input type="button" value="Generate" @click="startGen"
                           :disabled="running || inputError || error">
                </div>
                <div class="col-lg-6 col-sm-12">
                    <input type="button" value="Stop" @click="stopGen" :disabled="!running">
                </div>
            </div>
        </form>
    </div>
</template>

<script>
    export default {
        props: {
            running: Boolean,
            cores: Number,
        },
        data: function () {
            return {
                threads: 4,
                prefix: '',
                checksum: true,
                error: false
            }
        },
        computed: {
            inputError: function () {
                return !isValidHex(this.prefix);
            },
        },
        methods: {
            startGen: function () {
                this.$emit('start')
            },
            stopGen: function () {
                this.$emit('stop')
            }
        },
        watch: {
            prefix: function(){
                this.$emit('input-change', 'prefix', this.prefix);
            },
            checksum : function(){
                this.$emit('input-change', 'checksum', this.checksum);
            },
            threads: function(){
                this.$emit('input-change', 'threads', this.threads);
            },
        }
    }

    const isValidHex = function (hex) {
        return hex.length ? /^[0-9A-F]+$/g.test(hex.toUpperCase()) : true;
    };
</script>

<style lang="sass" scoped>
    @import "../css/variables"
    input
        &[type="text"]
            width: 100%
            color: $white-text
            background: $panel-background-clear
            outline: none
            font-size: 1.3em
            padding: 0.5em
            border: none
            margin-bottom: 10px
            -webkit-appearance: none
        &[type="button"]
            border: none
            outline: none
            color: $white-text
            padding: 0.6em
            font-size: 1.3em
            font-weight: 500
            margin: 1.3em 0 0 0
            cursor: pointer
            -webkit-appearance: none
            background: $teal
            width: 100%
            &:hover
                background: $yellow
            &:disabled
                background: $panel-background-clear
                cursor: auto
    .error-text
        display: none
        font-size: 0.85em
        color: $red
    .error
        input[type="text"]
            border: 1px solid $red
        .error-text
            display: block
    .check
        margin: .5em 0

    .checkbox
        margin-bottom: 4px
        padding-left: 30px
        line-height: 27px
        cursor: pointer
        position: relative
        font-size: 1.2em
        color: $white-text
        font-weight: 400
        &:last-child
            margin-bottom: 0
        i
            position: absolute
            bottom: 4px
            left: 17.5em
            display: block
            width: 19px
            height: 19px
            outline: none
            border: 1px solid $border-grey
            &.left
                position: absolute
                bottom: 4px
                left: 0
                display: block
                width: 19px
                height: 19px
                outline: none
                border: 1px solid $border-grey
        input
            + i:after
                content: ''
                background: url("../assets/images/tick-mark.png") no-repeat 1px 2px
                top: 4px
                left: 2px
                width: 15px
                height: 15px
                font: normal 12px/16px FontAwesome
                text-align: center
                position: absolute
                opacity: 0
            position: absolute
            left: -9999px
            &:checked + i:after
                opacity: 1

    .threads
        font-size: 1.2em
        h4
            display: inline
        input[type=button].square-btn
            display: inline-block
            width: 24px
            height: 24px
            margin: 0 5px 0 0
            padding: 0
            line-height: 1em

</style>