<template>
    <div id="header">
        <b-container>
            <b-row>
                <b-col sm="2" offset="5">
                    <img alt="Pokemon" src="../assets/logo-pokemon.png">
                </b-col>
            </b-row>
            <b-row id="search-bar">
                <b-col sm="8" offset="2">
                    <b-form-input
                        placeholder="Search by keywords"
                        v-model="keyword"
                        v-on:keyup="local"
                        v-on:keyup.enter="search"
                    ></b-form-input>
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>

<script>
export default {
    name: 'Header',
    props: {
        searchLocal: Function,
        searchApi: Function
    },
    data() {
        return {
            keyword: ''
        }
    },
    methods: {
        local() {
            this.searchLocal(this.keyword)
        },
        search() {
            if (this.keyword.length) {
                this.searchApi(this.keyword)
            }
            else {
                this.showModalAlert()
            }
        },
        showModalAlert() {
            this.$bvModal.msgBoxOk('This field is required', {
            title: 'Warning',
            size: 'sm',
            buttonSize: 'sm',
            okVariant: 'success',
            headerClass: 'p-2 border-bottom-0',
            footerClass: 'p-2 border-top-0',
            centered: true
            })
        }
    }
}
</script>

<style>
#header {
    margin: 30px 0;
}
#search-bar {
    margin-top:20px
}
</style>