<template>
  <div>
    <div class="content-input" id="input-content">
      <input
        v-model="text"
        @keydown="writedown($event)"
        @keyup="writeup($event)"
        @focus="is_show = true"
        class="input-txt"
        :class="inputClass"
        type="text"
        :placeholder="placeholder"
      />

      <ul
        class="ui-autocomplete"
        :class="{active:text!='' && is_show}"
      >
        <div v-if="text!='' && is_show" class="show-result">
          <li :key="idx" v-for="(dato,idx) of dataFiltered" class="ui-menu-item item-found">
            <a @click="onSelectItem(dato)" class="ui-item-result person" tabindex="-1">
              <span v-if="imgPhoto!=''" class="f-l mr-1 avatar">
                <img :src="imgPhoto" alt="person avatar" />
              </span>
              <div class="info-container">
                <div style="max-width: 150px;">
                  <span class="name">{{dato.name}}</span>
                  <div class="info-group">
                    <span v-if="dato.phone!=undefined" class="phone">
                      <span v-if="dato.phone">{{dato.phone}}</span>
                      <span v-else>No Phone Number</span>
                    </span>
                    <span v-if="dato.email!=undefined" class="email">{{dato.email}}</span>
                  </div>
                </div>
                <div class="info-group info-end">
                  <em v-if="dato.type" class="age-group">Adult</em>
                  <em class="membership"></em>
                  <em v-if="dato.addess" class="address">No Address</em>
                </div>
              </div>
              <div></div>
            </a>
          </li>
        </div>

        <li v-if="dataFiltered.length==0" class="ui-menu-item">
          <a class="ui-item-result person-not-found ui-menu-item-wrapper" tabindex="-1">
            <div class="text-helper">{{txtNotFound}}</div>
            <span class="icon-wrap"></span>
          </a>
        </li>

        <li v-if="showNewBotton" class="ui-menu-item">
          <a @click="onNewItem()" class="create-new-person btn">{{txtBtnNew}}</a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
const axios = require("axios");

export default {
  name: "vue-search",
  props: {
    placeholder: String,
    showNewBotton: {
      type:Boolean,
      default:true
    },
    searchByField: {
      type:Boolean,
      default:true
    },
    txtNotFound: {
      type: String,
      default: "No one found with that name."
    },
    txtBtnNew: {
      type: String,
      default: "Create A New Element"
    },
    inputClass: Object,
    model: String,
    imgPhoto: String,
    ApiSource: {
      type: String,
      default: ""
    },
    SourceField: {
      type: String,
      default: "name"
    },
    DataDefault: {
      type: Array,
      default: function() {
        return [];
      }
    }
  },
  data() {
    return {
      text: "",
      is_show: true,
      typingTimer: null,
      doneTypingInterval: 300,
      SourceData: [],
      dataFiltered: []
    };
  },
  mounted() {
    this.SourceData = this.DataDefault;
    document.addEventListener("click", (evt) => {
      var flyoutElement = document.getElementById("input-content"),
        targetElement = evt.target; // clicked element
      do {
        if (targetElement == flyoutElement) {
          return;
        }
        // Go up the DOM
        targetElement = targetElement.parentNode;
      } while (targetElement);

      // This is a click outside.
      this.is_show = false
    });
  },
  methods: {
    onNewItem: function() {
      this.$emit("newitem",this.text);
    },
    onSelectItem(dato) {
      this.is_show = false;
      this.text = dato.name;
      this.$emit("itemselected", dato);
    },
    writeup() {
      clearTimeout(this.typingTimer);
      this.typingTimer = setTimeout(this.doneTyping, this.doneTypingInterval);
    },
    writedown() {
      clearTimeout(this.typingTimer);
    },
    doneTyping() {
      if (this.ApiSource != "") {
        this.search();
      }
    },
    search() {
      let url = this.ApiSource;
      if(this.searchByField){
        url += "?" + this.SourceField + "=" + this.text
      }
      axios
        .get(url)
        .then(res => {
          this.SourceData = res.data;
          this.onFilter();
        });
    },
    onFilter() {
      let textFilter = this.text.toLowerCase();
      this.dataFiltered = [];
      this.is_show = true;
      for (let i = 0; i < this.SourceData.length; i++) {
        let record = this.SourceData[i][this.SourceField];
        if (record) {
          if (record.toLowerCase().indexOf(textFilter) > -1) {
            this.dataFiltered.push(this.SourceData[i]);
          }
        }
      }
    }
  },
  watch: {
    text(val) {
      this.onFilter();
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style  lang="scss">
.content-input {
  width: 100%;
  position: relative;

  // height: 200px;
  background: #866c6c;
  .input-txt {
    width: 100%;
    height: 33px;
    padding-top: 0.5em;
    padding-bottom: 0.5em;
    border-color: #dfe5f0;
    box-shadow: inset 0 1px 2px #ebebeb;
    line-height: normal;
    border-radius: 3px;
    position: relative;
    margin: 0;
    padding: 8px;
    width: 99.8%;
    vertical-align: middle;
    font: inherit;
    font-size: 100%;
    border: 1px solid #b0b0b0;
    box-shadow: inset 0 1px 2px #ebebeb;
    background: #ffffff;

    &:focus {
      box-shadow: 0 0 0 2px white;
      border: 1px solid #c2d6ff;
      outline: none;
    }
  }

  .ui-autocomplete {
    border-color: #e0e0e0;
    box-shadow: 0 1px 6px -1px rgba(64, 64, 64, 0.4);
    position: absolute;
    z-index: 1000;
    top: 33px;
    left: 0;
    right: 0;
    padding: 0;
    list-style-type: none;
    border-width: 1px;
    border-style: solid;
    border-top: 0;
    border-bottom-right-radius: 4px;
    border-bottom-left-radius: 4px;
    background: #fff;
    display: none;

    font-family: inherit;
    line-height: 1.6;
    margin: 0;
    list-style-position: outside;

    .show-result {
      max-height: 300px;
      overflow: hidden;
      overflow-y: scroll;
    }
    &.active {
      display: block;
    }
    .ui-menu-item {
      border-bottom-color: #dfe5f0;
      display: block;
      overflow: auto;
      font-size: 13px;
      padding: 0;
      border-bottom-width: 1px;
      border-bottom-style: solid;
      &.item-found {
        &:hover {
          cursor: pointer;
          background-color: #e4eaf5;
        }
      }
      .ui-item-result {
        .avatar {
          width: 24px;
          height: 24px;
          box-sizing: border-box;
          display: block;
          overflow: hidden;
          border-radius: 50%;
          width: 1.5em;
          height: 1.5em;
          float: left;
          img {
            width: 100%;
          }
        }
        .info-container {
          display: flex;
          flex-direction: row;
          justify-content: space-between;
          .name {
            display: block;
            font-size: 14px;
            margin-bottom: 4px;
          }
          .info-group {
            .email {
              max-width: 100%;
              white-space: nowrap;
              overflow: hidden;
              text-overflow: ellipsis;
            }
            .phone,
            .email,
            .age-group,
            .address,
            .membership {
              color: #7e7e7e;
              display: block;
              font-size: 12px;
            }
            &.info-end {
              display: flex;
              flex-wrap: wrap;
              flex-direction: column;
              align-self: flex-end;
            }
          }
        }
      }
      .person {
        padding-left: 0;
        margin-left: 8px;
        overflow: auto;
      }
      a {
        line-height: 1.35;
        position: relative;
        display: block;
        padding: 0.6em 0.75em;
        text-decoration: none;
        &.create-new-person {
          margin: 8px;
          padding: 5px 7.5px 7.5px;
          border-radius: 2px;
          background: #e8f0fd;
          border-color: #ccd2dd;
          font-size: 12px;
          cursor: pointer;
          &:focus {
            outline: none;
            box-shadow: none;
          }
          &:hover {
            background: #ccd2dd;
          }
        }
      }
    }
  }
}
</style>
