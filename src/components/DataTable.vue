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
    <p>{{mst}}</p>

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
              v-for="(item, key) in filteredItems"
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
                  <li
                    class="mdc-list-item mdc-list-item--selected"
                    aria-selected="true"
                    role="option"
                    data-value="10"
                  >
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
              1‑10 of 100
            </div>
            <button
              class="mdc-icon-button material-icons mdc-data-table__pagination-button"
              data-first-page="true"
              disabled
            >
              <div class="mdc-button__icon">first_page</div>
            </button>
            <button
              class="mdc-icon-button material-icons mdc-data-table__pagination-button"
              data-prev-page="true"
              disabled
            >
              <div class="mdc-button__icon">chevron_left</div>
            </button>
            <button
              class="mdc-icon-button material-icons mdc-data-table__pagination-button"
              data-next-page="true"
            >
              <div class="mdc-button__icon">chevron_right</div>
            </button>
            <button
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
import axios from 'axios';

export default {
  data() {
    return {
      dataTable: [],
      searchKey: "",
      selected: null,
      loading: true,
      errored: false
    };
  },
  computed: {
    filteredItems() {
      let items = [];
      items = this.dataTable.filter((item) => {
        return (
          item.name.toLowerCase().indexOf(this.searchKey.toLowerCase()) > -1
        );
      });
      return items;
    },
  },
  async created() {
    try {
      const res = await axios.get('http://localhost:3000/todos')
      this.dataTable = res.data;
    } catch(e) {
      console.error(e)
    }
  },
};
</script>

<style scoped>
</style>
