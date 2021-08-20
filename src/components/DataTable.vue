<template>
  <div class="mdc-layout-grid">
    <div class="row">
      <label class="mdc-text-field mdc-text-field--outlined">
        <span class="mdc-text-field__ripple"></span>
        <input
          class="mdc-text-field__input"
          v-model="searchKey"
          type="text"
          placeholder="Search"
          aria-label="Label"
        />
        <span class="mdc-line-ripple"></span>
      </label>
    </div>
    <br />

    <div class="mdc-data-table">
      <div class="mdc-data-table__table-container">
        <table class="mdc-data-table__table" aria-label="Dessert calories">
          <thead>
            <tr class="mdc-data-table__header-row">
              <th
                class="mdc-data-table__header-cell"
                role="columnheader"
                scope="col"
              >
                Id
              </th>
              <th
                class="mdc-data-table__header-cell"
                role="columnheader"
                scope="col"
              >
                Nome
              </th>
              <th
                class="mdc-data-table__header-cell"
                role="columnheader"
                scope="col"
              >
                Altura
              </th>
              <th
                class="mdc-data-table__header-cell"
                role="columnheader"
                scope="col"
              >
                Idade
              </th>
            </tr>
          </thead>
          <tbody class="mdc-data-table__content">
            <tr
              v-for="(item, key) in currentData"
              :key="key"
              class="mdc-data-table__row"
            >
              <th class="mdc-data-table__cell" scope="row">{{ item.id }}</th>
              <td class="mdc-data-table__cell">
                {{ item.name }}
              </td>
              <td class="mdc-data-table__cell">
                {{ item.altura }}
              </td>
              <td class="mdc-data-table__cell">
                {{ item.idade }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="mdc-data-table__pagination">
        <div class="mdc-data-table__pagination-trailing">
          <div class="mdc-data-table__pagination-rows-per-page">
            <div class="mdc-data-table__pagination-rows-per-page-label">
              Rows per page
            </div>

            <div
              class="mdc-select mdc-select--outlined mdc-select--no-label mdc-data-table__pagination-rows-per-page-select"
            >
              <div
                class="mdc-select__anchor"
                role="button"
                aria-haspopup="listbox"
                aria-labelledby="demo-pagination-select"
                tabindex="0"
              >
                <span class="mdc-select__selected-text-container">
                  <span
                    id="demo-pagination-select"
                    class="mdc-select__selected-text"
                    >10</span
                  >
                </span>
                <span class="mdc-select__dropdown-icon">
                  <svg
                    class="mdc-select__dropdown-icon-graphic"
                    viewBox="7 10 10 5"
                  >
                    <polygon
                      class="mdc-select__dropdown-icon-inactive"
                      stroke="none"
                      fill-rule="evenodd"
                      points="7 10 12 15 17 10"
                    ></polygon>
                    <polygon
                      class="mdc-select__dropdown-icon-active"
                      stroke="none"
                      fill-rule="evenodd"
                      points="7 15 12 10 17 15"
                    ></polygon>
                  </svg>
                </span>
                <span class="mdc-notched-outline mdc-notched-outline--notched">
                  <span class="mdc-notched-outline__leading"></span>
                  <span class="mdc-notched-outline__trailing"></span>
                </span>
              </div>

              <div
                class="mdc-select__menu mdc-menu mdc-menu-surface mdc-menu-surface--fullwidth"
                role="listbox"
              >
                <ul class="mdc-list">
                  <li class="mdc-list-item" role="option" data-value="10">
                    <span class="mdc-list-item__text">10</span>
                  </li>
                  <li class="mdc-list-item" role="option" data-value="25">
                    <span class="mdc-list-item__text">25</span>
                  </li>
                  <li class="mdc-list-item" role="option" data-value="100">
                    <span class="mdc-list-item__text">100</span>
                  </li>
                </ul>
              </div>
            </div>
          </div>

          <div class="mdc-data-table__pagination-navigation">
            <div class="mdc-data-table__pagination-total">
              {{ currentPage }} of {{ totalPage }}
            </div>
            <button
              :disabled="currentPage <= 1"
              @click="firstPage"
              class="mdc-icon-button material-icons mdc-data-table__pagination-button"
              data-first-page="true"
            >
              <div class="mdc-button__icon">first_page</div>
            </button>
            <button
              :disabled="currentPage <= 1"
              @click="goPrevious"
              class="mdc-icon-button material-icons mdc-data-table__pagination-button"
              data-prev-page="true"
            >
              <div class="mdc-button__icon">chevron_left</div>
            </button>
            <button
              :disabled="!hasMore"
              @click="goNext"
              class="mdc-icon-button material-icons mdc-data-table__pagination-button"
              data-next-page="true"
            >
              <div class="mdc-button__icon">chevron_right</div>
            </button>
            <button
              :disabled="!hasMore"
              @click="lastPage"
              class="mdc-icon-button material-icons mdc-data-table__pagination-button"
              data-last-page="true"
            >
              <div class="mdc-button__icon">last_page</div>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      dataTable: [],
      searchKey: "",
      selected: null,
      currentPage: 1,
      totalPage: 0,
      limit: 2,
      total: 0,
      newTableItens: 0,
    };
  },
  watch: {
    searchKey: function() {
      this.total = this.dataTable.length;
      this.newTableItens = this.dataTable.filter((item) => {
        return (
          item.name.toLowerCase().indexOf(this.searchKey.toLowerCase()) > -1
        );
      });
      this.total = this.newTableItens.length;

      if (this.total / this.limit > Math.round(this.total / this.limit)) {
        this.totalPage = Math.round(this.total / this.limit) + 1;
      } else {
        this.totalPage = Math.round(this.total / this.limit);
      }
    },
  },
  computed: {
    currentData() {
      let items = [];
      if (this.searchKey) {
        items = this.dataTable.filter((item) => {
          return (
            item.name.toLowerCase().indexOf(this.searchKey.toLowerCase()) > -1
          );
        });
        const start = this.currentPage * this.limit - this.limit;
        const end = start + this.limit;
        return items.slice(start, end);
      } else {
        const start = this.currentPage * this.limit - this.limit;
        const end = start + this.limit;
        return this.dataTable.slice(start, end);
      }
    },
    hasMore() {
      return this.currentPage < this.total / this.limit;
    },
  },
  methods: {
    goPrevious() {
      this.currentPage = this.currentPage - 1;
    },
    goNext() {
      this.currentPage = this.currentPage + 1;
    },
    firstPage() {
      this.currentPage = 1;
    },
    lastPage() {
      this.currentPage = this.totalPage;
    },
  },
  async created() {
    try {
      const res = await axios.get("http://localhost:3000/todos");
      // const res = await axios.get('http://localhost:3000/todos/?_page=1&_limit=20')
      this.dataTable = res.data;
      this.total = this.dataTable.length;

      if (this.total / this.limit > Math.round(this.total / this.limit)) {
        this.totalPage = Math.round(this.total / this.limit) + 1;
      } else {
        this.totalPage = Math.round(this.total / this.limit);
      }
    } catch (e) {
      console.error(e);
    }
  },
};
</script>
