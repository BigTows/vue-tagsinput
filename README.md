# Voerro Vue Tags Input v2 

This fork of [*Voerro Vue Tags*](https://github.com/voerro/vue-tagsinput) with experimental ideas and features.
> Thanks Alexander Zavyalov for work 

### Experimental ideas and features 
1. Added description field (available only for dropdown mode)
2. Added icon for result of tag (available only for dropdown mode)
### Demo
![](demo.gif)
![](demo3.gif)

[**Live Demo**](https://bigtows.github.io/vue-tagsinput/)

## Installation via NPM

```
npm i https://github.com/BigTows/vue-tagsinput --save-dev
```
or
```
npm i https://github.com/BigTows/vue-tagsinput --save
```

Then register the component with Vue:

```javascript
import VoerroTagsInput from '@voerro/vue-tagsinput';
import '@voerro/vue-tagsinput/dist/style.css'; // Or you can use custom css file based on this
Vue.component('tags-input', VoerroTagsInput);
```

## Usage

```html
<tags-input element-id="tags"
    v-model="selectedTags"
    :existing-tags="[
        { key: 'web-development', value: 'Web Development' },
        { key: 'php', value: 'PHP' },
        { key: 'javascript', value: 'JavaScript' },
    ]"
    :typeahead="true"></tags-input>
```

```html
<tags-input element-id="tags"
    v-model="selectedTags"
    :existing-tags="[
        { key: 1, value: 'Web Development' },
        { key: 2, value: 'PHP' },
        { key: 3, value: 'JavaScript' },
    ]"
    :typeahead="true"></tags-input>
```

`element-id` will be applied to `id` and `name` attributes of the hidden input that contains the list of the selected tags as its value. Optionally you can also use the `v-model` directive to bind a variable to the array of selected tags.

`existing-tags` is the list of the existing on your website tags. Include it even if you're not using typeahead.

Remove the `typeahead` property to disable this functionality.

#### Setting Selected Tags Programmatically

If you need to programmatically (manually) set or change the list of selected tags from "outside" - just set the required value to the variable bound with the component via `v-model`.

For example, the variable name is `selectedTags`:
```html
<tags-input element-id="tags" 
    v-model="selectedTags"></tags-input>
```

You can pre-set the value of this variable:
```javascript
new Vue({
    el: '#app',

    components: { VoerroTagsInput },

    data: {
        selectedTags: [
            { key: 'web-development', value: 'Web Development' },
            { key: 'php', value: 'PHP' },
            { key: 'javascript', value: 'JavaScript' },
        ],
    }
});
```

... or change it whenever you need to:
```javascript
new Vue({
    el: '#app',

    components: { VoerroTagsInput },

    data: {
        selectedTags: [],
    },

    methods: {
        setSelectedTags() {
            this.selectedTags = [{ key: 'php', value: 'PHP' }];
        }
    }
});
```

#### All Available Props

Prop | Type | Default | Description
--- | --- | --- | ---
elementId | String | - | id & name for the hidden input.
existing-tags | Array | [] | An array with existing tags in the following format: `[{ key: 'id-or-slug-of-the-tag', value: 'Tag\'s text representation' }, {...}, ...]`
typeahead | Boolean | false | Whether the typeahead (autocomplete) functionality should be enabled.
typeahead-style | String | 'badges' | The autocomplete prompt style. Possible values: `badges`, `dropdown`.
typeahead-max-results | Number | 0 | Maximum number of typeahead results to be shown. 0 - unlimited.
typeahead-activation-threshold | Number | 1 | Show typeahead results only after at least this many characters were entered. When set to 0, typeahead with all the available tags will be displayed on input focus.
placeholder | String | 'Add a tag' | The placeholder of the tag input.
limit | Number | 0 | Limit the number of tags that can be chosen. 0 = no limit.
only-existing-tags | Boolean | false | Only existing tags can be added/chosen. New tags won't be created.
case-sensitive-tags | Boolean | false | Determines whether tags are case sensitive. Setting this to `true` would allow tags like `php`, `PHP`, `PhP`, and so on to be added at the same time.
delete-on-backspace | Boolean | true | Whether deleting tags by pressing Backspace is allowed.
allow-duplicates | Boolean | false | Allow users to add the same tags multiple times.
validate | Function | `text => true` | Callback to validate tags' text with.
add-tags-on-comma | Boolean | false | Add new tags when comma is pressed. The search (typeahead) results are ignored.
add-tags-on-blur | Boolean | false | Add new tags when on the input is blur. The search (typeahead) results are ignored.
sort-search-results | Boolean | true | Whether the search results should be sorted.
before-adding-tag | Function | `tag => true` | Callback to perform additional checks and actions before a tag is added. Return `true` to allow a tag to be added or `false` to forbid the action.
before-removing-tag | Function | `tag => true` | Callback to perform additional checks and actions before a tag is removed. Return `true` to allow a tag to be added or `false` to forbid the action.

#### Events

Event | Description
--- | ---
@initialized | Fired when the component is completely ready to be worked with. Fired from the Vue.js' `mounted()` method.
@tag-added | Fired when a new tag is added. The slug of the tag is passed along.
@tag-removed | Fired when a tag is removed. The slug of the tag is passed along.
@tags-updated | Fired when a tag is added or removed.
@keydown | Fires on a keydown event
@keyup | Fires on a keyup event
@focus | Fired when the input is focused
@blur | Fired when the input is blurred

```html
<voerro-tags-input
    ...
    @initialized="onInitialized"
    @tag-added="onTagAdded"
    @tag-removed="onTagRemoved"
    @tags-updated="onTagsUpdated"
    @keydown="onKeyDown"
    @keyup="onKeyUp"
    @focus="onFocus"
    @blur="onBlur"
></voerro-tags-input>
```

```javascript
<script>
new Vue({
    ...

    methods: {
        onInitialized() {
            console.log('Initialized');
        },

        onTagAdded(slug) {
            console.log(`Tag added: ${slug}`);
        },

        onTagRemoved(slug) {
            console.log(`Tag removed: ${slug}`);
        },

        onTagsUpdated() {
            console.log('Tags updated');
        },

        onKeyDown() {
            console.log('Key down');
        },

        onKeyUp() {
            console.log('Key up');
        },

        onFocus() {
            console.log('Input focused');
        },

        onBlur() {
            console.log('Input blurred');
        },
    }
});
</script>
```

## Data

You can bind the array of selected tags to a variable via `v-model`. A tag object within the array looks like this:

```
{ key: 'web-development', value: 'Web Development' }
```

`key` is whatever unique key you use for the tags in your project. It could be a unique slug, it could be a unique numeric id, it could be something else. `value` is the text representation of a tag.

There's also a hidden text input, which has the stringified version of the array of selected tags as its value. The `name` and `id` of the input equal to whatever you set to the `element-id` prop.

The tags that don't exist in the `existing-tags` array will have its `key` equal to an empty string `''`. In your backend you can consider these tags as `to be created`.

## Styling

If you want to completely re-style the component - write your own styles from scratch using `dist/style.css` as a reference. Alternatively you can override specific parts of `dist/style.css` using `!important`.

Certain classes/styles can be overridden via component props on a per instance basis in case you just want to make minor changes, e.g. you just want to change colors.

Prop | Default class | Area
--- | --- | ---
wrapper-class | tags-input-wrapper-default | Outer appearance of the input - a wrapper providing a border and padding around the selected tags. If you're using CSS frameworks, you could use the frameworks' native classes, e.g. `form-control` for Bootstrap or `input` for Bulma.

## Using Typeahead (Autocomplete)

When search results are displayed underneath the input, use the `arrow down` and `arrow up` keys on the keyboard to move the selection. Press `Enter` to select a tag. Press `Esc` to discard the search results and then `Enter` to add a new tag the way you've typed it.

## Updating From Older Versions

#### Older versions up to v1.5.0 -> v1.5.1

See the `v1` branch for details.

#### v1.5.1 and above -> v2.*

A pretty serious bug (#53) was fixed in `v2.0.0`. The data format for the `existing-tags` prop and the `v-model` directive has been changed. You can find the new format in this documentation, see above.

## Contribution

Everyone is welcome to contribute. When making a contribution, please base your branch off of `dev` and merge it into `dev` as well. Thank you!

## Support

This software is absolutely free to use and is developed in the author's free time. If you found this software useful and would like to say thank you to the author, please consider making a donation. It's not the amount, it's the gesture.

- BTC (Bitcoin): 34ReKHWmSoGbcEHJw3HtyZ5CXz1UfoUKG5
- PayPal: https://paypal.me/AlexanderZavyalov
