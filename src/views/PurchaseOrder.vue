<template>
  <div class="purchase-order">
    <router-link to="/" class="back-btn">
      <el-button type="text" icon="el-icon-arrow-left">Dashboard</el-button>
    </router-link>
    <h1 class="title">
      Select items to add to the Purchase Order
    </h1>
    <div class="filters-table-container">
      <el-button plain icon="el-icon-caret-top" :class="visible ? 'button--color2' : 'button--empty'" @click="toggleFilters">{{filters}}</el-button>
      <div class="filters-table" v-if="visible">
        <div class="table-column">
          <div class="table-row">
            <div class="table-block table-block--wide">
              <span class="table-block-title">Request for proposals</span>
              <el-input placeholder="" v-model="requestForProposalsInput" class="table-input-element"></el-input>
              <span class="table-block-title">Creation date</span>
              <el-input placeholder="All dates" v-model="creationDateInput" class="table-input-element"></el-input>
              <el-button plain class="button--empty" @click="clearFilter">Reset filters</el-button>
            </div>
            <div class="table-block table-block--wide">
              <span class="table-block-title">Purchase requsition number</span>
              <el-input placeholder="" v-model="purchaseRequsitionNumberInput" class="table-input-element"></el-input>
              <span class="table-block-title">Delivery date</span>
              <el-input placeholder="" v-model="deliveryDateInput" class="table-input-element"></el-input>
            </div>
          </div>
        </div>
        <el-divider direction="vertical"></el-divider>
        <div class="table-column">
          <div class="table-row">
            <div class="table-block table-block--wide">
              <span class="table-block-title">Location</span>
              <el-select v-model="filtersLocationOptionsValue" placeholder="Select" class="table-input-element">
                <el-option
                  v-for="item in filtersLocationOptions"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
              <span class="table-block-title">Department</span>
              <el-select v-model="filtersDepartmentOptionsValue" placeholder="Select" class="table-input-element">
                <el-option
                  v-for="item in filtersDepartmentOptions"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                  >
                </el-option>
              </el-select>
            </div>
            <div class="table-block table-block--wide">
              <span class="table-block-title">Requester</span>
              <el-select v-model="filtersRequesterValue" placeholder="Select" class="table-input-element">
                <el-option
                  v-for="item in filtersRequesterOptions"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                  >
                </el-option>
              </el-select>
              <span class="table-block-title">Cost center</span>
              <el-select v-model="filtersCostCenterValue" placeholder="Select" class="table-input-element">
                <el-option
                  v-for="item in filtersCostCenterOptions"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                  >
                </el-option>
              </el-select>
              <div class="button-container">
                <el-button type="text" @click="saveFilter">Save filters</el-button>
              </div>
            </div>
          </div>
        </div>
        <div class="table-column table-column--last">
          <div class="table-row">
            <div class="table-block table-block--narrow">
              <span class="table-block-title">Total</span>
              <el-input placeholder="from" v-model="fromValue" class="table-input-element"></el-input>
            </div>
            <div class="table-block table-block--narrow">
              <span class="table-block-title">UAH</span>
              <el-input placeholder="to" v-model="toValue" class="table-input-element"></el-input>
            </div>
          </div>
          <el-button plain class="button--empty button--color">Filter</el-button>
        </div>
      </div>
      <div class="filters-table-container--inner">
        <el-form ref="form" :model="form" label-width="auto">
          <el-form-item label="Categories" class="filters-table-checkbox">
            <el-checkbox-group v-model="form.checkList">
              <el-checkbox label="All" name="checkList" class="filters-table-checkbox"></el-checkbox>
              <el-checkbox label="Products" name="checkList" class="filters-table-checkbox"></el-checkbox>
              <el-checkbox label="New Product" name="checkList" class="filters-table-checkbox"></el-checkbox>
            </el-checkbox-group>
          </el-form-item>
        </el-form>
        <el-form ref="form2" :model="form2" label-width="auto">
            <el-form-item label="Items" class="filters-table-checkbox">
              <el-checkbox v-model="checked">All</el-checkbox>
            </el-form-item>
          </el-checkbox-group>
        </el-form>
      </div>
    </div>
    <h1 class="title">
      FIll out the details for this Purchase Order
    </h1>
    <div class="main-table-container">
      <div class="table-column">
        <div class="table-row">
          <div class="table-block table-block--wide">
            <span class="table-block-title">Delivery date</span>
            <el-date-picker
              v-model="dateValue"
              type="date"
              placeholder="dd.mm.yyyy"
              class="table-input-element"
              >
            </el-date-picker>
            <span class="table-block-title2">Supplier</span>
            <el-link type="primary" class="table-input-element2" :underline="underline">Amazon</el-link>
            <span class="table-block-title">Department</span>
            <el-select v-model="departmentOptionsValue" placeholder="Select" class="table-input-element">
              <el-option
                v-for="item in departmentOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value"
                >
              </el-option>
            </el-select>
          </div>
          <div class="table-block table-block--wide">
            <span class="table-block-title">Location</span>
            <el-select v-model="locationOptionsValue" placeholder="Select" class="table-input-element">
              <el-option
                v-for="item in locationOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
            <span class="table-block-title2">Currency</span>
            <el-link type="primary" class="table-input-element2" :underline="underline">USD</el-link>
            <span class="table-block-title">Payment method</span>
            <el-select v-model="paymentMethodValue" placeholder="Select" class="table-input-element">
              <el-option
                v-for="item in paymentMethodOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </div>
        </div>
        <span class="table-block-title">Budgets</span>
        <el-input
          resize="none"
          type="textarea"
          :rows="3"
          placeholder="to view Budgets select:
- Delivery date, Location
- Department, Cost center, Project
"
          class="table-input-element"
          v-model="budgetsTextarea"
          >
        </el-input>
        <div class="table-row table-row--center">
          <div class="table-block table-block--wide">
            <el-button plain class="button--empty button--color" @click="console">Create</el-button>
          </div>
          <div class="table-block">
            <div>Total: 2,100.00 UAH</div>
          </div>
        </div>
      </div>
      <div class="table-column">
        <div class="table-row">
          <div class="table-block table-block--wide">
            <span class="table-block-title">Legal entity</span>
            <el-input placeholder="select Location first" v-model="legalEntityInput" class="table-input-element"></el-input>
            <span class="table-block-title2">Terms of Payment</span>
            <el-link type="primary" class="table-input-element2" :underline="underline">Prepayment</el-link>
            <span class="table-block-title">Cost center</span>
            <el-select v-model="costCenterValue" placeholder="Select" class="table-input-element">
              <el-option
                v-for="item in costCenterOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value"
                >
              </el-option>
            </el-select>
          </div>
          <div class="table-block table-block--wide">
            <span class="table-block-title">Taxes</span>
            <el-select v-model="taxesValue" multiple placeholder="Select" class="table-input-element">
              <el-option
                v-for="item in taxesOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value"
                >
              </el-option>
            </el-select>
            <span class="table-block-title2">Supplier</span>
            <el-link type="primary" class="table-input-element2" :underline="underline"># 20304 - Precoro Inc.</el-link>
            <span class="table-block-title">Project</span>
            <el-select v-model="projectValue" placeholder="Select" class="table-input-element">
              <el-option
                v-for="item in projectOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </div>
        </div>
        <span class="table-block-title">Note</span>
        <el-input
          type="textarea"
          resize="none"
          :rows="3"
          placeholder="Write your note..."
          v-model="noteTextarea"
          class="table-input-element"
          >
        </el-input>
      </div>
    </div>
    <div class="currencies-table-container">
      <div class="table-column">
        <div class="table-row">
          <div class="table-block">
            <div class="currencies-table-header">
              <span class="table-block-title">Exchange rate</span>
              <span class="table-block-title table-block-title--clickable" icon="el-icon-caret-top" @click="visibleCurrencies = !visibleCurrencies">+</span>
            </div>
            <div class="table-row" v-if="visibleCurrencies">
              <div class="table-block table-block--wide">
                <span class="table-block-title">USD:UAH</span>
                <el-input type="number" placeholder="" v-model="USDUAHInput" class="table-input-element"></el-input>
              </div>
              <div class="table-block table-block--wide">
                <span class="table-block-title">USD:EUR</span>
                <el-input type="number" placeholder="" v-model="USDEURInput" class="table-input-element"></el-input>
              </div>
              <div class="table-block table-block--wide">
                <span class="table-block-title">USD:VND</span>
                <el-input type="number" placeholder="" v-model="USDVNDInput" class="table-input-element"></el-input>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'purchase-order',
  components: {
  },
  data() {
    return {
      visibleCurrencies: true,
      USDUAHInput: 0,
      USDEURInput: 0,
      USDVNDInput: 0,
      filters: "Show filters",
      underline: false,
      visible: false,
      form: {
        checkList: ['All','Products', 'New Product']
      },
      form2: {
        checkList: ['All']
      },
      checked: true,
      dateValue: '',
      departmentOptions: [{
          value: 'CSM',
          label: 'CSM'
        }, {
          value: 'MSC',
          label: 'MSC'
        }],
      locationOptions: [{
          value: '---',
          label: '---'
        }, {
          value: 'Ukraine',
          label: 'Ukraine'
        }],
      paymentMethodOptions: [{
          value: 'Bank Transfer',
          label: 'Bank Transfer'
        }, {
          value: 'Western Union',
          label: 'Western Union'
        }],
      costCenterOptions: [{
          value: 'Posters',
          label: 'Posters'
        }, {
          value: 'Posters2',
          label: 'Posters2'
        }],
      taxesOptions: [{
          value: 'Vat 20%',
          label: 'Vat 20%'
        }, {
          value: 'Vat 10%',
          label: 'Vat 10%'
        }],
      projectOptions: [{
          value: 'Website',
          label: 'Website'
        }, {
          value: 'Website2',
          label: 'Website2'
        }],
      departmentOptionsValue: '',
      locationOptionsValue: '',
      paymentMethodValue: '',
      costCenterValue: '',
      taxesValue: '',
      projectValue: '',
      budgetsTextarea: '',
      noteTextarea: '',
      legalEntityInput: '',
      requestForProposalsInput: '',
      creationDateInput: '',
      purchaseRequsitionNumberInput: '',
      deliveryDateInput: '',
      filtersLocationOptionsValue: '',
      filtersLocationOptions: [{
          value: 'all',
          label: 'all'
        }, {
          value: 'Ukraine',
          label: 'Ukraine'
        }],
      filtersDepartmentOptionsValue: '',
      filtersDepartmentOptions: [{
          value: 'CSM',
          label: 'CSM'
        }, {
          value: 'MSC',
          label: 'MSC'
        }],
      filtersRequesterValue: '',
      filtersRequesterOptions: [{
          value: 'CSM',
          label: 'CSM'
        }, {
          value: 'MSC',
          label: 'MSC'
        }],
      filtersCostCenterValue: '',
      filtersCostCenterOptions: [{
          value: 'Posters',
          label: 'Posters'
        }, {
          value: 'Posters2',
          label: 'Posters2'
        }],
      fromValue: '',
      toValue: '',
      filter: {
        requestForProposalsInput: '',
        creationDateInput: '',
        purchaseRequsitionNumberInput: '',
        deliveryDateInput: '',
        filtersLocationOptionsValue: '',
        filtersDepartmentOptionsValue: '',
        filtersRequesterValue: '',
        filtersCostCenterValue: '',
        fromValue: '',
        toValue: ''
      },
      loadedFilter: {}
    }
  },
  created() {
    this.USDUAHInput = 25
    this.USDEURInput = 26
    this.USDVNDInput = 250
    this.costCenterValue = "Posters"
    this.departmentOptionsValue = "CSM"
    this.paymentMethodValue = "Bank Transfer"
    this.locationOptionsValue = "---"
    this.projectValue = "Website"
    this.deliveryDateInput = "May 11 – May 28"
    this.loadedFilter = JSON.parse(localStorage.getItem('filterParams'))
    if(this.loadedFilter) {
      this.loadedFilter = JSON.parse(localStorage.getItem('filterParams'))
      this.requestForProposalsInput = this.loadedFilter.requestForProposalsInput || ''
      this.creationDateInput = this.loadedFilter.creationDateInput || ''
      this.purchaseRequsitionNumberInput = this.loadedFilter.purchaseRequsitionNumberInput || ''
      this.deliveryDateInput = this.loadedFilter.deliveryDateInput || ''
      this.filtersLocationOptionsValue = this.loadedFilter.filtersLocationOptionsValue || ''
      this.filtersDepartmentOptionsValue = this.loadedFilter.filtersDepartmentOptionsValue || ''
      this.filtersRequesterValue = this.loadedFilter.filtersRequesterValue || ''
      this.filtersCostCenterValue = this.loadedFilter.filtersCostCenterValue || ''
      this.fromValue = this.loadedFilter.fromValue || ''
      this.toValue = this.loadedFilter.toValue || ''
    }
  },
  methods: {
    clearFilter() {
      localStorage.clear()
      this.requestForProposalsInput = this.creationDateInput = this.purchaseRequsitionNumberInput = this.deliveryDateInput = this.filtersLocationOptionsValue = this.filtersDepartmentOptionsValue = this.filtersRequesterValue = this.filtersCostCenterValue = this.fromValue = this.toValue = ''
    },
    saveFilter() {
      this.filter = {
        requestForProposalsInput: this.requestForProposalsInput,
        creationDateInput: this.creationDateInput,
        purchaseRequsitionNumberInput: this.purchaseRequsitionNumberInput,
        deliveryDateInput: this.deliveryDateInput,
        filtersLocationOptionsValue: this.filtersLocationOptionsValue,
        filtersDepartmentOptionsValue: this.filtersDepartmentOptionsValue,
        filtersRequesterValue: this.filtersRequesterValue,
        filtersCostCenterValue: this.filtersCostCenterValue,
        fromValue: this.fromValue,
        toValue: this.toValue
      }
      localStorage.setItem('filterParams', JSON.stringify(this.filter))
      this.loadedFilter = JSON.parse(localStorage.getItem('filterParams'))
    },
    console() {
      console.log(this.dateValue + ' ' + this.departmentOptionsValue + ' ' + this.locationOptionsValue + ' ' + this.paymentMethodValue)
    },
    toggleFilters() {
      if (this.filters === "Show filters") {
        this.filters = "Hide filters"
        this.visible = true
      } else {
        this.filters = "Show filters"
        this.visible = false
      }
    }
  }
}
</script>
<style scoped>
  .back-btn {
    text-decoration: none;
    font-size: 12px;
    line-height: 14px;
  }
  .purchase-order {
    padding: 24px;
    width: 100%;
  }
  .title {
    color: #4E5361;
  }
  .filters-table-container,
  .filters-table-container--inner {
    display: flex;
    justify-content: space-between;
  }
  .filters-table-container--inner {
    background: #CDDAEA;
    border-radius: 10px;
    align-items: center;
    height: 50px;
    padding-left: 16px;
  }
  .button--empty,
  .button--empty:hover,
  .button--empty:focus {
    height: 50px;
    background: transparent;
    border: 2px solid #B4CCE6;
    border-radius: 10px;
    color: #5093E1;
  }
  .button--color2,
  .button--color2:hover,
  .button--color2:focus {
    border: none;
    height: 50px;
    background: #CDDAEA;
    border-radius: 10px 10px 0 0;
    color: #5093E1;
    outline: none;
  }
  .filters-table-checkbox {
    margin: 0;
    line-height: unset;
  }
  .main-table-container {
    background: white;
    display: flex;
    padding: 24px;
    border-radius: 10px;
    position: relative;
    margin-bottom: 50px;
  }
  .table-column {
    display: flex;
    flex-direction: column;
  }
  .table-column:not(:last-child) {
    display: flex;
    flex-direction: column;
    margin-right: 22px;
  }
  .table-row {
    display: flex;
    flex-direction: row;
  }
  .currencies-table-container {
    background: #CDDAEA;
    display: flex;
    padding: 24px;
    border-radius: 10px;
    margin-bottom: 50px;
    width: 734px;
    /* width: fit-content; */
  }
  .table-block {
    display: flex;
    flex-direction: column;
  }
  .table-block:not(:last-child) {
    margin-right: 22px;
  }
  .table-block-title {
    margin-bottom: 8px;
  }
  .table-block-title2 {
    margin-bottom: 15px;
  }
  .table-block-title,
  .table-block-title2 {
    font-size: 15px;
    line-height: 18px;
    color: #7C869C;
  }
  .table-block--wide {
    width: 230px;
  }
  .table-block--narrow {
    width: 110px;
  }
  .table-input-element {
    margin-bottom: 22px;
  }
  .table-input-element2 {
    margin-bottom: 28px;
    justify-content: flex-start;
  }
  .table-row--center {
    align-items: center;
  }
  .button--color {
    background: #5093E1;
    color: white;
    border: none;
  }
  .filters-table {
    position: absolute;
    top: 194px;
    background: #CDDAEA;
    border-radius: 0 10px 10px 10px;
    z-index: 1000;
    display: flex;
    padding: 24px;
  }
  .table-block-title--clickable {
    cursor: pointer;
  }
  .table-column--last {
    justify-content: space-between;
  }
  .button-container {
    height: 50px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
  }
  .currencies-table-header {
    display: flex;
    justify-content: space-between;
    width: 734px;
  }
</style>