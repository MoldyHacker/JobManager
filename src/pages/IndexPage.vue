<template>
  <q-page class="flex flex-center">
    <q-list bordered class="rounded-borders" style="max-width: 600px">
      <!-- <q-item-label header>Google Inbox style</q-item-label> -->

      <q-item v-for="order in orders" :key="order.orderId">
        <!-- <q-item-section avatar top>
          <q-icon name="account_tree" color="black" size="34px" />
        </q-item-section> -->

        <q-item-section top class="col-2 gt-sm">
          <q-item-label class="q-mt-sm">{{ order.jobNumber }}</q-item-label>
        </q-item-section>

        <q-item-section top class="col-2 gt-sm">
          <q-item-label class="q-mt-sm">{{ order.department }}</q-item-label>
        </q-item-section>

        <q-item-section top>
          <q-item-label lines="1">
            <span class="text-weight-medium">{{ order.customer }}</span>
            <span class="text-grey-8"> - {{ order.customerPO }}</span>
          </q-item-label>
          <q-item-label caption lines="1">
            {{ order.partNumber }} - {{ order.quantity }}pcs
          </q-item-label>
          <q-item-label
            lines="1"
            class="q-mt-xs text-body2 text-weight-bold text-primary text-uppercase"
          >
            <span class="cursor-pointer">Open in GitHub</span>
          </q-item-label>
        </q-item-section>

        <q-item-section top side>
          <div class="text-grey-8 q-gutter-xs">
            <q-btn class="gt-xs" size="12px" flat dense round icon="delete" />
            <q-btn class="gt-xs" size="12px" flat dense round icon="done" />
            <q-btn size="12px" flat dense round icon="more_vert" />
          </div>
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script setup>
const orders = [
  {
    orderId: 1,
    customer: 'SonoSite',
    customerPO: 'JM2454',
    department: 'Surface Grinding',
    jobNumber: '39254',
    material: 'Rexolite',
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
    dueDate: '02-01-2025',
    partDescription: 'Matching Layer Plate. Rexolite RC350',
    partNumber: 'P21018-01 Rev A',
    quantity: 95,
    status: 'Pending Processing',
  },
  {
    orderId: 3,
    customer: 'SonoSite',
    customerPO: 'JM2454',
    department: 'Surface Grinding',
    jobNumber: '39254',
    material: 'Rexolite',
    dueDate: '12-03-2024',
    manufacturingCompletionDate: '12-01-2024',
    partDescription: 'Matching Layer Plate. Rexolite RC350',
    partNumber: 'P21018-01 Rev A',
    quantity: 105,
    status: 'Turned In',
  },
  {
    orderId: 4,
    customer: 'SonoSite',
    customerPO: 'JM2454',
    department: 'Surface Grinding',
    jobNumber: '39254',
    material: 'Rexolite',
    dueDate: '11-05-2024',
    manufacturingCompletionDate: '11-05-2024',
    partDescription: 'Matching Layer Plate. Rexolite RC350',
    partNumber: 'P21018-01 Rev A',
    quantity: 95,
    status: 'Shipped',
    shippedDate: '11-06-2024',
  },
  {
    orderId: 5,
    customer: 'SonoSite',
    customerPO: 'JM2425',
    department: 'Surface Grinding',
    jobNumber: '39253',
    material: 'Rexolite',
    dueDate: '10-05-2024',
    manufacturingCompletionDate: '10-01-2024',
    partDescription: 'Matching Layer Plate. Rexolite RC350',
    partNumber: 'P22181-01 Rev A',
    quantity: 90,
    status: 'Completed',
    shippedDate: '10-02-2024',
    completedDate: '10-02-2024',
  },
]
</script>
