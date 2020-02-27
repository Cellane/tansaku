<template>
  <div>
    <header
      class="header"
      @mouseover="hover = true"
      @mouseleave="hover = false"
    >
      <h1 class="header-title">
        <a href="/">{{ hoverText }}</a>
      </h1>
    </header>

    <div class="container">
      <ais-instant-search :search-client="searchClient" index-name="articles">
        <ais-configure v-bind="settings" />

        <div class="search-panel">
          <div class="search-panel__filters">
            <ais-clear-refinements />
            <h3>Difficulty level</h3>
            <ais-refinement-list attribute="level" />
            <h3>Lesson type</h3>
            <ais-refinement-list attribute="type" />
          </div>

          <div class="search-panel__results">
            <ais-search-box class="searchbox" />

            <ais-hits>
              <template slot="item" slot-scope="{ item }">
                <h1>
                  <a
                    :href="item.link"
                    v-if="item.link && item.type !== 'drills'"
                  >
                    <ais-highlight :hit="item" attribute="title" />
                  </a>
                  <ais-highlight :hit="item" attribute="title" v-else />
                </h1>
                <p>
                  <ais-highlight :hit="item" attribute="perex" />
                </p>
              </template>
            </ais-hits>

            <div class="pagination">
              <ais-pagination />
            </div>
          </div>
        </div>
      </ais-instant-search>
    </div>
  </div>
</template>

<script>
import algoliasearch from "algoliasearch/lite"
import "instantsearch.css/themes/algolia-min.css"

export default {
  data() {
    return {
      searchClient: algoliasearch(
        "VSY0Z7WWKQ",
        "98437b36db2be6055a91c94177bba4e9"
      ),
      settings: {
        hitsPerPage: 16,
        disjunctiveFacetsRefinements: {
          type: ["lesson"]
        }
      },
      hover: false
    }
  },

  computed: {
    hoverText() {
      if (this.hover) {
        return "日本語　探索 🇯🇵🕵️‍♂️"
      }

      return "Nihongo Tansaku 🇯🇵🕵️‍♂️"
    }
  }
}
</script>

<style>
body,
h1 {
  margin: 0;
  padding: 0;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica,
    Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
}

.header {
  display: flex;
  align-items: center;
  min-height: 50px;
  padding: 0.5rem 1rem;
  background-image: linear-gradient(to right, #4dba87, #2f9088);
  color: #fff;
  margin-bottom: 1rem;
}

.header a {
  color: #fff;
  text-decoration: none;
}

.header-title {
  font-size: 1.2rem;
  font-weight: normal;
}

.header-title::after {
  padding: 0 0.5rem;
}

.header-subtitle {
  font-size: 1.2rem;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 1rem;
}

.search-panel {
  display: flex;
}

.search-panel__filters {
  flex: 1;
  margin-right: 1em;
}

.search-panel__results {
  flex: 3;
}

.searchbox {
  margin-bottom: 2rem;
}

.pagination {
  margin: 2rem auto;
  text-align: center;
}
</style>
