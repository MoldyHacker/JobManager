<template>
  <div class="">
    <q-btn label="Reset Filters" />
    <!-- sort by due date or job number -->
    <q-select
      v-model="sortBy"
      :options="sortOptions"
      label="Sort by"
      @update:model-value="sortOrders"
    />
    <!-- filter by status -->
    <q-select
      v-model="selectedStatus"
      :options="statusOptions"
      label="Filter by Production Status"
      multiple
      use-chips
    />
    <!-- filter by department -->
    <q-select
      v-model="selectedDepartment"
      :options="departmentOptions"
      label="Filter by Department"
    />
    <!-- filter by customer -->
    <q-select v-model="selectedCustomer" :options="customerOptions" label="Filter by Customer" />
  </div>
  <q-page class="flex flex-center">
    <q-list bordered separator class="rounded-borders">
      <q-item class="items-center">
        <q-item-section top class="col-1">
          <q-item-label class="text-weight-medium">Job #</q-item-label>
        </q-item-section>

        <q-item-section top class="col-1 gt-sm">
          <q-item-label class="text-weight-medium" lines="2">Customer</q-item-label>
        </q-item-section>
        <q-item-section top class="col gt-sm">
          <q-item-label class="text-grey-8">PO #</q-item-label>
        </q-item-section>

        <q-item-section top class="col lt-md">
          <q-item-label class="text-weight-medium">Customer</q-item-label>
          <q-item-label class="text-grey-8">PO #</q-item-label>
        </q-item-section>

        <q-item-section top class="col-3">
          <q-item-label class="text-weight-medium">Part Number</q-item-label>
          <q-item-label class="text-grey-8">Qty</q-item-label>
        </q-item-section>

        <q-item-section top class="col-auto" no-wrap>
          <q-item-label class="text-weight-medium">Due Date</q-item-label>
        </q-item-section>
        <!-- <q-space /> -->

        <q-item-section top class="col text-right gt-xs">
          <q-item-label class="text-weight-medium">Status</q-item-label>
          <q-item-label class="text-grey-8">Department</q-item-label>
        </q-item-section>

        <q-item-section top side class="col">
          <div class="text-grey-8 q-gutter-xs">Actions</div>
        </q-item-section>
      </q-item>
      <q-item
        v-for="order in filteredOrders"
        :key="order.orderId"
        class="items-center"
        :disable="order.status !== 'Pending Production' && order.status !== 'In Progress'"
      >
        <q-item-section top class="col-1">
          <q-item-label class="text-weight-medium">{{ order.jobNumber }}</q-item-label>
        </q-item-section>

        <q-item-section top class="col-1 gt-sm">
          <q-item-label class="text-weight-medium" lines="2">{{ order.customer }}</q-item-label>
        </q-item-section>
        <q-item-section top class="col gt-sm">
          <q-item-label class="text-grey-8">{{ order.customerPO }}</q-item-label>
        </q-item-section>

        <q-item-section top class="col lt-md">
          <q-item-label class="text-weight-medium ellipsis">{{ order.customer }}</q-item-label>
          <q-item-label class="text-grey-8 ellipsis">{{ order.customerPO }}</q-item-label>
        </q-item-section>

        <q-item-section top class="col-3">
          <q-item-label class="text-weight-medium ellipsis">{{ order.partNumber }}</q-item-label>
          <q-item-label class="text-grey-8">{{ order.quantity }}pcs</q-item-label>
        </q-item-section>

        <q-item-section top class="col-auto" no-wrap>
          <q-item-label class="">{{ order.newDueDate }}</q-item-label>
          <q-item-label v-if="order.newDueDate" class="text-strike">{{
            order.dueDate
          }}</q-item-label>
          <q-item-label v-else class="">{{ order.dueDate }}</q-item-label>
        </q-item-section>

        <!-- <q-space /> -->

        <q-item-section top class="col text-right gt-xs">
          <q-item-label class="text-weight-medium ellipsis">{{ order.status }}</q-item-label>
          <q-item-label class="text-grey-8">{{ order.department }}</q-item-label>
        </q-item-section>

        <q-item-section top side class="col">
          <div class="text-grey-8 q-gutter-xs">
            <!-- <q-btn class="gt-xs" size="12px" flat dense round icon="delete" /> -->
            <q-btn class="gt-xs" size="12px" flat dense round icon="done" />
            <q-btn class="lt-sm" size="12px" flat dense round icon="more_vert" />
          </div>
        </q-item-section>
      </q-item>
      <q-item v-if="filteredOrders.length === 0">
        <q-item-section top class="items-center">
          <q-item-label class="text-weight-medium">No Data...</q-item-label>
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script setup>
import { ref, computed } from 'vue'

const sortBy = ref('Due Date')
const sortOptions = ref(['Due Date', 'Job Number'])

const selectedStatus = ref(['In Progress', 'Pending Production'])
const statusOptions = ref([
  'In Progress',
  'Pending Production',
  'Production Complete',
  'Shipped',
  'Completed',
])

const selectedDepartment = ref('All')
const departmentOptions = ref(['All', 'Surface Grinding', 'CNC Milling', 'CNC Turning'])

const selectedCustomer = ref('All')
const customerOptions = ref(['All', 'SonoSite', 'GE Medical Systems China', 'Materion'])

const filteredOrders = computed(() => {
  return orders.value.filter((order) => {
    const statusMatch =
      selectedStatus.value.length === 0 || selectedStatus.value.includes(order.status)
    const departmentMatch =
      selectedDepartment.value === 'All' || order.department === selectedDepartment.value
    const customerMatch =
      selectedCustomer.value === 'All' || order.customer === selectedCustomer.value
    return statusMatch && departmentMatch && customerMatch
  })
})

const sortOrders = () => {
  if (sortBy.value === 'Job Number') {
    orders.value.sort((a, b) => a.jobNumber - b.jobNumber)
  } else if (sortBy.value === 'Due Date') {
    orders.value.sort((a, b) => {
      const [monthA, dayA, yearA] = a.dueDate.split('-')
      const dateA = new Date(yearA, monthA - 1, dayA)

      const [monthB, dayB, yearB] = b.dueDate.split('-')
      const dateB = new Date(yearB, monthB - 1, dayB)

      return dateA - dateB
    })
  }
}

const orders = ref([
  {
    orderId: 1,
    customer: 'SonoSite Fujifilm',
    customerPO: 'JM2454',
    department: 'Surface Grinding',
    jobNumber: '39254',
    material: 'Rexolite',
    dateReceived: '11-01-2024',
    dueDate: '01-01-2025',
    newDueDate: '12-15-2024',
    partDescription: 'Matching Layer Plate. Rexolite RC350',
    partNumber: 'P21018-01 Rev A',
    quantity: 100,
    status: 'In Progress',
    assignedTo: 'John Doe',
  },
  {
    orderId: 2,
    customer: 'SonoSite',
    customerPO: 'JM2454',
    department: 'Surface Grinding',
    jobNumber: '39254',
    material: 'Rexolite',
    dateReceived: '11-01-2024',
    dueDate: '02-01-2025',
    partDescription: 'Matching Layer Plate. Rexolite RC350',
    partNumber: 'P21018-01 Rev A',
    quantity: 95,
    status: 'Pending Production',
  },
  {
    orderId: 3,
    customer: 'SonoSite',
    customerPO: 'JM2454',
    department: 'Surface Grinding',
    jobNumber: '39254',
    material: 'Rexolite',
    dateReceived: '11-01-2024',
    dueDate: '12-03-2024',
    manufacturingCompletionDate: '12-01-2024',
    partDescription: 'Matching Layer Plate. Rexolite RC350',
    partNumber: 'P21018-01 Rev A',
    quantity: 105,
    status: 'Production Complete',
  },
  {
    orderId: 4,
    customer: 'GE Medical Systems China',
    customerPO: 'JM2454',
    department: 'Surface Grinding',
    jobNumber: '35113',
    material: 'ABS',
    dateReceived: '11-01-2024',
    dueDate: '11-05-2024',
    manufacturingCompletionDate: '11-05-2024',
    partDescription: 'ML2 Universal Linear Array',
    partNumber: '5432305 Rev 001',
    quantity: 400,
    status: 'Shipped',
    shippedDate: '11-06-2024',
  },
  {
    orderId: 5,
    customer: 'Materion',
    customerPO: 'JM2425',
    department: 'Surface Grinding',
    jobNumber: '39253',
    material: 'ABS',
    dateReceived: '8-01-2024',
    dueDate: '10-05-2024',
    manufacturingCompletionDate: '10-01-2024',
    partDescription: 'Matching Layer Plate',
    partNumber: 'E-1.09x0.89-846-01-GI-1 Rev 001',
    quantity: 500,
    status: 'Completed',
    shippedDate: '10-02-2024',
    completedDate: '10-02-2024',
  },
  {
    orderId: 6,
    customer: 'GE Healthcare Japan',
    department: 'Surface Grinding',
    jobNumber: '35162',
    material: 'Rexolite',
    dateReceived: '11-01-2024',
    dueDate: '1-28-2019',
    partDescription: 'Matching Layer #2 RAB-D200',
    partNumber: '5786576 Rev 1',
    quantity: 20,
    status: 'Completed',
  },
])

sortOrders()
</script>

<style lang="scss">
.q-list {
  width: 90%;
}
</style>
