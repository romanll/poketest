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
                    <b-form-input placeholder="Search by keywords"></b-form-input>
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>

<script>
export default {
    name: 'Header'
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