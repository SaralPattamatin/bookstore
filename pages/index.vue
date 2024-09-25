<template>
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 py-8">
    <div
      v-for="(product, index) in products"
      :key="index"
      class="max-w-sm mx-auto bg-white shadow-md rounded-lg overflow-hidden"
    >
      <!-- รูปสินค้า -->
      <img
        :src="product.image"
        :alt="product.name"
        class="w-full h-48 object-cover"
      />

      <!-- ข้อมูลสินค้า -->
      <div class="p-6">
        <h2 class="text-xl font-semibold text-gray-800">{{ product.name }}</h2>
        <p class="text-gray-600 mt-2">{{ product.description }}</p>
        <p class="text-lg font-bold text-gray-900 mt-4">{{ product.price }}</p>

        <!-- ปุ่ม Add to Cart -->
        <button
          class="mt-4 bg-blue-500 text-white px-4 py-2 rounded-lg hover:bg-blue-600 transition"
          @click="addToCart(product)"
        >
          Add to Cart
        </button>
      </div>
    </div>
  </div>

  <div>
    <div class="fixed bottom-4 right-4 z-20">
      <button
        @click="(showCart = !showCart), calculateDiscount()"
        class="bg-blue-500 text-white px-4 py-4 rounded-full"
      >
        <img :src="cartIcon" alt="Icon" class="h-[20px] w-[20px]" />
        <span
          class="absolute top-0 left-9 w-5 h-5 bg-red-700 rounded-full flex items-center justify-center text-white"
        >
          {{ cart.length }}
        </span>
      </button>
    </div>

    <div
      v-if="showCart"
      class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50"
    >
      <div class="bg-white rounded-lg shadow-lg p-6 w-96">
        <h2 class="text-2xl font-bold mb-4">Shopping Cart</h2>
        <div v-if="cart.length > 0">
          <ul>
            <li
              v-for="(item, index) in cart"
              :key="index"
              class="flex justify-between items-center mb-4"
            >
              <span>{{ item.name }}</span>
              <span>{{ item.price }} บาท</span>
            </li>
          </ul>
          <p class="text-xl font-semibold mt-4 text-right">
            ราคารวมก่อนหักส่วนลด: {{ totalBeforeDiscount }} บาท
          </p>
          <p class="text-xl font-semibold mt-2 text-green-500 text-right">
            ส่วนลด: {{ discount }} บาท
          </p>
          <p class="text-xl font-bold mt-4 text-right">
            ราคาสุทธิ: {{ calculateDiscountedPrice(cart).discountedPrice }} บาท
          </p>
        </div>
        <p v-else class="text-gray-500">Your cart is empty.</p>
        <div class="flex justify-end mt-4">
          <button
            @click="showCart = false"
            class="mt-4 bg-red-500 text-white px-4 py-2 rounded-lg"
          >
            Close
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import Image1 from "~/assets/images/hp1.jpg";
import Image2 from "~/assets/images/hp2.jpg";
import Image3 from "~/assets/images/hp3.jpg";
import Image4 from "~/assets/images/hp4.jpg";
import Image5 from "~/assets/images/hp5.jpg";
import Image6 from "~/assets/images/hp6.jpg";
import Image7 from "~/assets/images/hp7.jpg";
import cartIcon from "~/assets/icons/cart-icon.svg";

const products = ref([
  {
    name: "แฮร์รี่ พอตเตอร์ 1",
    description: "This is a short description of product 1.",
    price: 100,
    image: Image1,
    id: 1,
  },
  {
    name: "แฮร์รี่ พอตเตอร์ 2",
    description: "This is a short description of product 2.",
    price: 100,
    image: Image2,
    id: 2,
  },
  {
    name: "แฮร์รี่ พอตเตอร์ 3",
    description: "This is a short description of product 3.",
    price: 100,
    image: Image3,
    id: 3,
  },
  {
    name: "แฮร์รี่ พอตเตอร์ 4",
    description: "This is a short description of product 4.",
    price: 100,
    image: Image4,
    id: 4,
  },
  {
    name: "แฮร์รี่ พอตเตอร์ 5",
    description: "This is a short description of product 5.",
    price: 100,
    image: Image5,
    id: 5,
  },
  {
    name: "แฮร์รี่ พอตเตอร์ 6",
    description: "This is a short description of product 6.",
    price: 100,
    image: Image6,
    id: 6,
  },
  {
    name: "แฮร์รี่ พอตเตอร์ 7",
    description: "This is a short description of product 7.",
    price: 100,
    image: Image7,
    id: 7,
  },
]);

const cart = ref([]);
const conts = ref([0, 0, 0, 0, 0, 0, 0, 0]);
const discount = ref(0);
const showCart = ref(false);

function addToCart(product) {
  cart.value.push(product);

  if (product.id >= 0 && product.id < conts.value.length) {
    conts.value[product.id]++;
  }
  calculateDiscount();
}

const totalBeforeDiscount = computed(() =>
  cart.value.reduce((sum, item) => sum + item.price, 0)
);

function calculateDiscountedPrice(books) {
  const pricePerBook = 100; // ราคาหนังสือแต่ละเล่ม

  let totalBooks = books.length;
  let uniqueBooks = new Set(books).size; // จำนวนหนังสือที่ไม่ซ้ำกัน
  let totalPrice = totalBooks * pricePerBook;

  if (uniqueBooks >= 2 && uniqueBooks <= 7) {
    let discountRate = discount.value;
    let discountedPrice = totalPrice - discountRate;
    return { discountedPrice };
  }
  return { discountedPrice: totalPrice, discountAmount: 0 }; // กรณีที่ไม่มีส่วนลด
}

function calculateDiscount() {
  discount.value = 0;
  let flag = true;

  const currentConts = [...conts.value];

  while (flag) {
    let count_uni = 0;

    for (let i = 0; i < currentConts.length; i++) {
      if (currentConts[i] !== 0) {
        count_uni++;
      }
    }

    if (count_uni > 0) {
      discount.value += (count_uni - 1) * 10 * count_uni;
    }

    for (let i = 0; i < currentConts.length; i++) {
      if (currentConts[i] !== 0) {
        currentConts[i]--;
      }
    }

    flag = false;
    for (let i = 0; i < currentConts.length; i++) {
      if (currentConts[i] !== 0) {
        flag = true;
        break;
      }
    }
  }
}
</script>
