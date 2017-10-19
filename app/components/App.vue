<template>
    <!-- The HTML template for our component -->
    <div id="app">
        <sidebar :categories="categories"
                 v-on:category-selected="setSelectedCategory">
            <!-- bind 'selected-category event to the event handler setSelectedCategory' -->
        </sidebar>

        <bookmark-list :bookmarks="bookmarks | filterByCategory selectedCategory"
                       :categories="categories">

        </bookmark-list>
    </div>
</template>

<script>
    // the Javascript for our component
    // We will export a Vue component options object here
    import store from './store'
    import Sidebar from './components/Sidebar.vue'
    import BookmarkList from './components/BookmarkList.vue'
    import { filterByCategory } from './filters'

    export default {

        components: {
            Sidebar,
            BookmarkList
        },

        data () {
            return {
                categories: {},
                bookmarks: {},
                selectedCategory: ''
            }
        },

        filters: {
            filterByCategory
        },

        created () {
            // assign the event handler `updateListings` to the `data-updated` event
            store.on('data-updated', this.updateListings)
        },

        methods: {
            // set the bookmarks and categories data properties to the new ones
            // received from the store
            updateListings (categories, bookmarks) {
                this.categories = categories
                this.bookmarks = bookmarks
            },

            setSelectedCategory (category) {
                this.selectedCategory = category;
            }

        }

    }
</script>
