<script>
import axios from 'axios';

export default {
    data() {
        return {
            randomUser: null,
            name: null,
            distribution_center: null,
            product: [],
            unit: [],
            price: [],
            quantity: [],
            productsOrder: [],
            totalproductsOrder: [],
            total: 0,
            totalPrice: 0,
            paymentTypeList: [
                {
                    id: 1,
                    name: 'Cash H+1',
                },
                {
                    id: 2,
                    name: 'Cash H+3',
                },
                {
                    id: 3,
                    name: 'Cash H+7',
                },
                {
                    id: 4,
                    name: 'Transfer H+1',
                },
                {
                    id: 5,
                    name: 'Transfer H+3',
                },
                {
                    id: 6,
                    name: 'Transfer H+7',
                },
            ],
            productsList: [
                {
                    id: 1,
                    product_name: 'Morning Dev Milk',
                    units: [
                        {
                            name: 'Karton',
                            price: 150000,
                        },
                        {
                            name: 'Pak',
                            price: 15000,
                        },
                        {
                            name: 'Pcs',
                            price: 1500,
                        },
                    ],
                },
                {
                    id: 2,
                    product_name: 'Le Minerale 600ml',
                    units: [
                        {
                            name: 'Karton',
                            price: 100000,
                        },
                        {
                            name: 'Pak',
                            price: 10000,
                        },
                        {
                            name: 'Pcs',
                            price: 1000,
                        },
                    ],
                },
                {
                    id: 3,
                    product_name: 'Greenfields Full Cream Milk 1L',
                    units: [
                        {
                            name: 'Karton',
                            price: 200000,
                        },
                        {
                            name: 'Pak',
                            price: 20000,
                        },
                        {
                            name: 'Pcs',
                            price: 2000,
                        },
                    ],
                },
            ],
        };
    },
    methods: {
        insertProduct(id, unitId, productId) {
            const product = this.productsList.find(
                (product) => product.id === productId,
            );

            const order = this.productsOrder.find((order) => order.id === id);

            if (order) {
                this.productsOrder[id].product_name = product.product_name;
                this.productsOrder[id].unit = this.unit;
                this.productsOrder[id].price = product.units[unitId].price;
                this.productsOrder[id].quantity = this.quantity;
            }

            if (this.price[id]) {
                this.price[id] = product.units[unitId].price;
            } else {
                this.price.push(product.units[unitId].price);
            }
            console.log(this.productsOrder);
        },

        calculateTotal(id) {
            let total = this.quantity[id] * this.price[id];
            this.totalproductsOrder[id] = total;

            this.calculateTotalPrice();
        },

        calculateTotalPrice() {
            let totalPrice = 0;
            for (let i = 0; i < this.totalproductsOrder.length; i++) {
                totalPrice += this.totalproductsOrder[i];
            }
            return (this.totalPrice = totalPrice);
        },

        addProductItem(id) {
            const product = this.productsOrder.find(
                (product) => product.id === id,
            );
            if (!product) {
                this.productsOrder.push({
                    id: id,
                    product_name: '',
                    unit: '',
                    price: '',
                    quantity: '',
                });
            }
            console.log(product);
            console.log(this.productsOrder);
        },

        deleteProductItem(id) {
            this.productsOrder.splice(id, 1);
            this.product.splice(id, 1);
            this.unit.splice(id, 1);
            this.price.splice(id, 1);
            this.quantity.splice(id, 1);
            this.totalproductsOrder.splice(id, 1);
            this.calculateTotalPrice();
            console.log(this.productsOrder);
        },

        reset() {
            this.productsOrder = [];
            this.product = [];
            this.unit = [];
            this.price = [];
            this.quantity = [];
            this.totalproductsOrder = [];
            this.totalPrice = 0;
            this.addProductItem(0);
        },
    },
    mounted() {
        axios
            .get('https://random-data-api.com/api/users/random_user?size=10')
            .then((response) => (this.randomUser = response.data));
    },
};
</script>

<template>
    <div class="max-w-6xl p-3 mx-auto my-3 rounded-md bg-slate-200">
        <h1 class="pl-5 mb-2 font-bold">Create Order</h1>
        <form class="px-5 py-3 bg-white rounded-md">
            <div class="flex">
                <div class="w-1/4">
                    <h2 class="font-medium">Detail</h2>
                </div>
                <div class="flex flex-col w-3/4">
                    <div class="flex flex-col">
                        <label>Name<span class="text-red-600">*</span></label>
                        <select v-model="name" class="w-1/3 input" required>
                            <option
                                v-for="user in randomUser"
                                :key="user.id"
                                value="1"
                            >
                                {{ user.first_name }}
                            </option>
                        </select>
                    </div>
                    <div class="flex flex-col mt-10">
                        <label
                            >Distribution Center<span class="text-red-600"
                                >*</span
                            ></label
                        >
                        <select
                            v-model="distribution_center"
                            v-on:change="addProductItem(0)"
                            class="input"
                            required
                        >
                            <option v-if="!name" disabled value="">
                                No data available
                            </option>
                            <option v-if="name" value="1">DC Tangerang</option>
                            <option v-if="name" value="2">DC Cikarang</option>
                        </select>
                    </div>
                    <div
                        v-if="distribution_center"
                        class="flex mt-10 space-x-4"
                    >
                        <div class="w-1/2">
                            <label
                                >Payment Type<span class="text-red-600"
                                    >*</span
                                ></label
                            >
                            <select class="input" required>
                                <option
                                    v-for="payment in paymentTypeList"
                                    :key="payment.id"
                                    value=""
                                >
                                    {{ payment.name }}
                                </option>
                            </select>
                        </div>
                        <div class="w-1/2">
                            <label
                                >Expired Date<span class="text-red-600"
                                    >*</span
                                ></label
                            >
                            <input type="date" class="input" required />
                        </div>
                    </div>
                    <div v-if="distribution_center" class="flex flex-col mt-10">
                        <label>Notes</label>
                        <textarea class="input" rows="7"></textarea>
                    </div>
                </div>
            </div>

            <hr v-if="distribution_center" class="my-10" />

            <div v-if="distribution_center" class="flex">
                <div class="w-1/4">
                    <h2 class="font-medium">Products</h2>
                </div>
                <div class="flex flex-col w-3/4">
                    <div
                        v-for="(productsOrder, i) in productsOrder"
                        :key="i"
                        class="mt-5"
                    >
                        <div class="flex space-x-5">
                            <div class="w-3/4">
                                <label
                                    >Product<span class="text-red-600"
                                        >*</span
                                    ></label
                                >
                                <div class="">
                                    <button
                                        v-if="i > 0"
                                        v-on:click="deleteProductItem(i)"
                                        type="button"
                                        class="mr-2 -ml-7"
                                    >
                                        <svg
                                            xmlns="http://www.w3.org/2000/svg"
                                            viewBox="0 0 512 512"
                                            class="w-5 h-5 text-red-500"
                                        >
                                            <path
                                                d="M0 256C0 114.6 114.6 0 256 0C397.4 0 512 114.6 512 256C512 397.4 397.4 512 256 512C114.6 512 0 397.4 0 256zM175 208.1L222.1 255.1L175 303C165.7 312.4 165.7 327.6 175 336.1C184.4 346.3 199.6 346.3 208.1 336.1L255.1 289.9L303 336.1C312.4 346.3 327.6 346.3 336.1 336.1C346.3 327.6 346.3 312.4 336.1 303L289.9 255.1L336.1 208.1C346.3 199.6 346.3 184.4 336.1 175C327.6 165.7 312.4 165.7 303 175L255.1 222.1L208.1 175C199.6 165.7 184.4 165.7 175 175C165.7 184.4 165.7 199.6 175 208.1V208.1z"
                                                fill="currentColor"
                                            />
                                        </svg>
                                    </button>
                                    <select v-model="product[i]" class="input">
                                        <!-- <option
                                            v-if="product[i]"
                                            value=""
                                        ></option> -->
                                        <option
                                            v-for="product in productsList"
                                            :key="product.id"
                                            :value="product.id"
                                        >
                                            {{ product.product_name }}
                                        </option>
                                    </select>
                                </div>
                            </div>
                            <div class="w-1/4">
                                <label
                                    >Unit<span class="text-red-600"
                                        >*</span
                                    ></label
                                >
                                <select
                                    v-model="unit[i]"
                                    v-on:change="
                                        insertProduct(i, unit[i], product[i])
                                    "
                                    class="input"
                                >
                                    <option v-if="!product[i]" disabled>
                                        Not Available
                                    </option>

                                    <option v-if="product[i]" value="0">
                                        Karton
                                    </option>
                                    <option v-if="product[i]" value="1">
                                        Pak
                                    </option>
                                    <option v-if="product[i]" value="2">
                                        Pcs
                                    </option>
                                </select>
                            </div>
                        </div>

                        <div class="flex mt-10 space-x-5">
                            <div class="w-1/4">
                                <label
                                    >Quantity<span class="text-red-600"
                                        >*</span
                                    ></label
                                >
                                <input
                                    v-model="quantity[i]"
                                    v-on:change="calculateTotal(i)"
                                    type="number"
                                    class="input"
                                />
                            </div>
                            <div class="w-1/4">
                                <label
                                    >Price<span class="text-red-600"
                                        >*</span
                                    ></label
                                >
                                <input
                                    v-model="price[i]"
                                    type="text"
                                    class="input"
                                    disabled
                                />
                            </div>
                            <div class="w-2/4">
                                <div class="text-right">
                                    <label
                                        >Total Price<span class="text-red-600"
                                            >*</span
                                        ></label
                                    >
                                </div>
                                <input
                                    v-model="totalproductsOrder[i]"
                                    type="text"
                                    class="text-right input"
                                    disabled
                                />
                                <hr
                                    class="my-4 bg-gray-300 border border-gray-300"
                                />
                                <div class="flex justify-between font-medium">
                                    <h2>Total Net Price</h2>
                                    <h2>{{ totalproductsOrder[i] ?? 0 }}</h2>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div v-if="distribution_center" class="flex flex-col">
                <div class="flex justify-end w-2/5 pr-5 my-3">
                    <button
                        v-on:click="
                            addProductItem(
                                productsOrder[productsOrder.length - 1].id + 1,
                            )
                        "
                        type="button"
                        class="px-5 py-2 text-sm font-medium text-white uppercase transition bg-yellow-400 rounded-md shadow-lg hover:bg-yellow-500"
                    >
                        tambah item +
                    </button>
                </div>
                <div class="flex justify-end my-3">
                    <div
                        class="flex justify-between w-2/5 pl-12 text-lg font-medium"
                    >
                        <h2>Total</h2>
                        <h2>{{ totalPrice }}</h2>
                    </div>
                </div>
            </div>

            <hr v-if="distribution_center" class="my-5" />

            <div class="flex justify-end mt-5 space-x-5">
                <button
                    v-on:click="reset"
                    type="button"
                    class="px-5 py-2 text-sm font-medium uppercase transition bg-gray-100 rounded-md shadow-lg hover:bg-red-500 hover:text-white"
                >
                    cancel
                </button>
                <button
                    class="px-5 py-2 text-sm font-medium text-white uppercase transition rounded-md shadow-lg bg-emerald-400 hover:bg-emerald-500"
                >
                    confirm
                </button>
            </div>
        </form>
    </div>
</template>
