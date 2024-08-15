<template>
    <div>
        <div v-for="item in items" :key="item.barcode" class="barcode-item">
            <p class="region">{{ item.regionName }}</p>
            <p v-if="item.departmentName" class="department">{{ item.departmentName }}</p>
            <svg :id="'barcode-' + item.barcode"></svg>
            <p class="category">{{ item.categoryName }}</p>
        </div>
        <button @click="printBarcodes">Print Barcodes</button>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import JsBarcode from 'jsbarcode';

// Data for testing
const items = ref([
    { barcode: 'barcode_2345', regionName: 'Bosh Prokuratura', categoryName: 'Printer', departmentName: '15-bo\'lim' },
    { barcode: '987654321098', regionName: 'Samarqand v. Prokuraturasi', categoryName: 'Skanner' },
    { barcode: '112233445566', regionName: 'Buxoro v. Kogon t. Prokuraturasi', categoryName: 'Kompyuter' },
    { barcode: '556677889900', regionName: 'Harbiy Prokuratura', categoryName: 'Faks Aparat' },
    { barcode: '998877665544', regionName: 'Termiz Harbiy Prokuraturasi', categoryName: 'Proyektor' },
]);

onMounted(() => {
    generateBarcodes();
});

function generateBarcodes() {
    items.value.forEach(item => {
        JsBarcode(`#barcode-${item.barcode}`, item.barcode, {
            format: 'CODE128',
            font: 'monospace',
        });
    });
}

function printBarcodes() {
    window.print();
}
</script>

<style scoped>
/* General styling for the barcode item */
.barcode-item {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100%;
    width: 100%;
    margin: 0;
    padding: 0;
    page-break-before: always; /* Ensure each barcode starts on a new page */
}

/* Hide non-print elements */
@media print {
    /* Hide buttons, headers, or any other non-barcode content */
    body * {
        visibility: hidden;
    }

    .barcode-item, .barcode-item * {
        visibility: visible;
    }

    .barcode-item {
        page-break-before: always;
        width: 100%;
        height: 100%;
        margin: 0;
        padding: 0;
    }

    /* Set page size to landscape and remove browser default header/footer */
    @page {
        size: A4 landscape; /* Set the page size to A4 in landscape orientation */
        margin: 0; /* Remove default margins to eliminate headers/footers */
    }

    /* Ensure barcode content stretches to fill the page */
    .barcode-item {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        height: 100vh; /* Full height of the page */
        width: 100vw;  /* Full width of the page */
        box-sizing: border-box;
        margin: 0;
        padding: 0;
    }

    /* Style the SVG to take up as much space as possible while maintaining aspect ratio */
    svg {
        width: 80%; /* Adjust the width as needed */
        height: auto; /* Maintain aspect ratio */
    }

    /* Style text elements with larger font size, bold, and uppercase */
    .region, .category, .department {
        font-size: 48px; /* Larger font size */
        font-weight: bold; /* Bold text */
        font-family: sans-serif;
        text-transform: uppercase; /* Uppercase text */
        margin: 10px 0; /* Spacing between lines */
    }

    /* Ensure department name is styled consistently */
    .department {
        margin-top: 0; /* Remove margin-top for department name */
    }
}
</style>
