<template>
  <div>
      <h1>Schema Creator</h1>
      <h2>Content</h2>
      <div class="content select-box" @mouseup="getSelection" @keyup="getSelection">
        <p>Childish Gambino ‘This is America’  https://www.youtube.com/watch?v=VYOjWnS4cMY</p>
        <p>The Road Not Taken by Robert Frost 
            https://www.theparisreview.org/blog/2015/09/11/the-most-misread-poem-in-america/</p>
        <p>Jordan Peele’s ‘Get Out’  https://www.imdb.com/title/tt5052448/</p>
        <p>Susan Fowler’s ‘Reflecting On One Very, Very Strange Year At Uber’ 
            https://www.susanjfowler.com/blog/2017/2/19/reflecting-on-one-very-strange-year-at-uber</p>
        <textarea>
            How do we work on modern web development projects when software development is an 
            inherently ‘wicked problem’. I’m primarily interested in Collaboration as being the 
            best way of solving wicked problems (as opposed to competitive or authoritative) 
            but we don’t seem to focus much on successful companies that implement good collaborative 
            practices (preferring to focus on authoritative especially - see Jeff Bezos, Steve Jobs)
        </textarea>
      </div>
        <h2>Schemas</h2>
        <label for="schema-select">Schema Type:</label>

        <select id="schema-select" v-model="selected">
            <option value="">Select an option</option>
            <option v-for="option in options" v-bind:value="option.value">
                {{ option.text }}
            </option>
        </select>
        <p>Schema type: {{ selected }}</p>
        <p>Name is {{ currentSchema.name }}</p>
        <h4>Properties</h4>
        <div v-for="value, name in currentSchema.properties">
            <label class="label">{{name}}</label>
            <p>
                <input class="input" type="text" 
                    v-model="currentSchema.properties[name]" 
                    :placeholder="name+' value'">
                <button @click="removeNewProperty(name)">&times;</button>
            </p>
        </div>
        <p>
            <input @keyup.enter="addNewProperty" class="input" type="text" v-model="newPropertyName" 
                placeholder="Add new property">
            <button @click="addNewProperty">+</button>
        </p>
  </div>
</template>

<script>
    import Vue from 'vue'

    export default {
        name:'Schema',
        data() {
            return {
                selected: '',
                selectedRange: null,
                newPropertyName: '',
                options: [
                    {text:'Person', value: 'person'},
                    {text:'Event', value: 'event'},
                    {text:'Article', value: 'article'},
                    {text:'Recipe', value: 'recipe'},
                    {text:'Book', value: 'book'},
                    {text:'Product', value: 'product'}
                ],
                schemas: [],
                currentSchema: {
                    type: this.selected,
                    name: '',
                    properties: {}
                }
            }  
        },
        methods:{
            addNewProperty() {
                this.$set(this.currentSchema.properties, this.newPropertyName, '')
                this.newPropertyName = ''
            },
            removeNewProperty (name) {
                this.$delete(this.currentSchema.properties, name)
            },
            getSelectedText() {
                if (window.getSelection) {
                    let sel = window.getSelection()
                    if (sel.getRangeAt && sel.rangeCount) {
                        this.selectedRange = sel.getRangeAt(0);
                    }
                    return window.getSelection().toString()
                } else if (document.selection) {
                    this.selectedRange = document.selection.createRange()
                    return document.selection.createRange().text
                }
                return ''
            },
            getSelection(){
                let text = this.getSelectedText();
                if (this.selectedRange && text) {
                    console.log(text);
                    this.currentSchema.name = text;
                }
            }
        }
     }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>