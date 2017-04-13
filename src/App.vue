<template>
    <div id="app">
        <h1>Vue.js + webGL composer</h1>
        <webgl-composer class="shards" background="#ff5454" zoom=1.0 x_offset=0.0 y_offset=0.0 fov=70 v-html="children"> </webgl-composer>
        <div id="actions">
            <button v-on:click="add">New Item</button>
            <button v-on:click="clear">Clear</button>
        </div>
        <div v-for="node in childNodes">
            <hr>
            <ul>
                <li>
                    <h3> Node: {{ node.id }}</h3></li>
                <li>explode duration:
                    <input type="range" min="0.1" step="0.1" v-model="node.explodeDur" v-on:change="render"> [{{ node.explodeDur }}] </li>
                <li>explode delay:
                    <input type="range" step="0.1" v-model="node.explodeDel" v-on:change="render"> [{{ node.explodeDel }}] </li>
                <li>spin duration:
                    <input type="range" min="0.1" step="0.1" v-model="node.spinDur" v-on:change="render"> [{{ node.spinDur }}] </li>
                <li>spin delay:
                    <input type="range" step="0.1" v-model="node.spinDel" v-on:change="render"> [{{ node.spinDel }}] </li>
                <li>fade duration:
                    <input type="range" min="0.1" step="0.1" v-model="node.fadeDur" v-on:change="render"> [{{ node.fadeDur }}] </li>
                <li>fade delay:
                    <input type="range" step="0.1" v-model="node.fadeDel" v-on:change="render"> [{{ node.fadeDel }}] </li>

                <li>x offset:
                    <input type="range" min="-10" step="0.1" max="10" v-model="node.x_offset" v-on:change="render"> [{{ node.x_offset }}] </li>
                <li>y offset:
                    <input type="range" min="-10" step="0.1" max="10" v-model="node.y_offset" v-on:change="render"> [{{ node.y_offset }}] </li>

                <br>
                <li>source: {{ node.src }}</li>
                <li>
                    <select v-model="node.src" v-on:change="render">
                        <option value="./lyninx.svg">lyninx Text</option>
                        <option value="./lyninx_inkscape.svg">lyninx Logo</option>
                        <option value="./vue.svg">Vue Logo</option>
                    </select>
                </li>
                <br>
                <li>color: {{ node.color }}</li>
                <li>
                    <input type="color" v-model="node.color" v-on:change="render"> </li>
                <br> </ul>
        </div>
    </div>
</template>
<script>
export default {
    name: 'app',
    data() {
        return {
            children: '',
            childNodes: []
        }
    },
    methods: {
        render: function() {
            this.children = ''
            for (var i = this.childNodes.length - 1; i >= 0; i--) {
                var node = this.childNodes[i]
                this.children += `
            <c-layer id="` + node.id + `" src="` + node.src + `" color="` + node.color + `" z_depth="0.0" x_offset="` + node.x_offset + `" y_offset="` + node.y_offset + `" scale="1.0">
            <c-animation type="explode" duration="` + node.explodeDur + `" delay="` + node.explodeDel + `" looping="true"></c-animation>
            <c-animation type="spin" duration="` + node.spinDur + `" delay="` + node.spinDel + `" looping="true"></c-animation>
            <c-animation type="fade-in" duration="` + node.fadeDur + `" delay="` + node.fadeDel + `" looping="true"></c-animation>
            </c-layer>`
            }
        },
        add: function() {
            var node = {
                "id": this.childNodes.length,
                "src": "./lyninx.svg",
                "color": "#FFFFFF",
                "explodeDur": "4.0",
                "explodeDel": "4.0",
                "spinDur": "4.0",
                "spinDel": "4.0",
                "fadeDur": "4.0",
                "fadeDel": "0",
                "x_offset":"0",
                "y_offset":"0"
            }
            this.childNodes.push(node)
            this.render()
        },
        clear: function() {
            this.children = ''
            this.childNodes = []
        }
    }
}
</script>
<style lang="scss">
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: left;
}

#actions {
    padding: 20px;
    text-align: center;
}

body {
    overflow-y: scroll;
}

h1,
h2 {
    text-align: center;
    font-weight: normal;
}

ul {
    list-style-type: none;
    padding: 0;
}

button {
    background-color: #ff5454;
    border: none;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
}

li {
    margin: 0 10px;
}

a {
    color: #42b983;
}
</style>
