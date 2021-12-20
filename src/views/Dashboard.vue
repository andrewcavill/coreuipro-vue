<template>
  <div>
    <CRow>
      <CCol :md="6">
        <CCard class="mb-4">
          <CCardHeader> Financial History </CCardHeader>
          <CCardBody>
            <CTable align="middle" class="mb-0 border" hover responsive>
              <CTableHead color="light">
                <CTableRow>
                  <CTableHeaderCell>Date</CTableHeaderCell>
                </CTableRow>
              </CTableHead>
              <CTableBody>
                <CTableRow>
                  <CTableDataCell>10/11/2021</CTableDataCell>
                </CTableRow>
              </CTableBody>
            </CTable>
          </CCardBody>
        </CCard>
      </CCol>
    </CRow>
    <CRow>
      <CCol :md="6">
        <CCard class="mb-4">
          <CCardHeader> Financial History </CCardHeader>
          <CCardBody>
            <CSmartTable
              clickableRows
              :tableProps="{
                striped: true,
                hover: true,
              }"
              :tableHeadProps="{
                color: 'danger',
              }"
              :activePage="1"
              footer
              header
              :items="items"
              :columns="columns"
              columnFilter
              tableFilter
              cleaner
              itemsPerPageSelect
              :itemsPerPage="5"
              columnSorter
              pagination
            >
              <template #status="{ item }">
                <td>
                  <CBadge :color="getBadge(item.status)">
                    {{ item.status }}
                  </CBadge>
                </td>
              </template>
              <template #show_details="{ item, index }">
                <td class="py-2">
                  <CButton
                    color="primary"
                    variant="outline"
                    square
                    size="sm"
                    @click="toggleDetails(item, index)"
                  >
                    {{ Boolean(item._toggled) ? 'Hide' : 'Show' }}
                  </CButton>
                </td>
              </template>
              <template #details="{ item }">
                <CCollapse :visible="this.details.includes(item._id)">
                  <CCardBody>
                    <h4>
                      {{ item.username }}
                    </h4>
                    <p class="text-muted">User since: {{ item.registered }}</p>
                    <CButton size="sm" color="info" class="">
                      User Settings
                    </CButton>
                    <CButton size="sm" color="danger" class="ml-1">
                      Delete
                    </CButton>
                  </CCardBody>
                </CCollapse>
              </template>
            </CSmartTable>
          </CCardBody>
        </CCard>
      </CCol>
    </CRow>
  </div>
</template>

<script>
export default {
  name: 'AppDashboard',
  data: () => {
    return {
      columns: [
        {
          key: 'name',
          _style: { width: '40%' },
          _props: { color: 'primary', className: 'fw-semibold' },
        },
        'registered',
        { key: 'role', filter: false, sorter: false, _style: { width: '20%' } },
        { key: 'status', _style: { width: '20%' } },
        {
          key: 'show_details',
          label: '',
          _style: { width: '1%' },
          filter: false,
          sorter: false,
          _props: { color: 'primary', className: 'fw-semibold' },
        },
      ],
      details: [],
      items: [
        {
          name: 'John Doe',
          registered: '2018/01/01',
          role: 'Guest',
          status: 'Pending',
        },
        {
          name: 'Samppa Nori',
          registered: '2018/01/01',
          role: 'Member',
          status: 'Active',
          _props: {
            color: 'primary',
            align: 'middle',
          },
        },
      ],
    }
  },
  methods: {
    getBadge(status) {
      switch (status) {
        case 'Active':
          return 'success'
        case 'Inactive':
          return 'secondary'
        case 'Pending':
          return 'warning'
        case 'Banned':
          return 'danger'
        default:
          'primary'
      }
    },
    toggleDetails(item) {
      if (this.details.includes(item._id)) {
        this.details = this.details.filter((_item) => _item !== item._id)
        return
      }
      this.details.push(item._id)
    },
  },
  components: {},
  // setup() {},
}
</script>
