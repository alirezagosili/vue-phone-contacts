<script setup>
import { ref, computed } from "vue";

const searchQuery = ref("");
const contacts = ref([]);

const filteredContacts = computed(() => {
    if (searchQuery.value.trim() === "") {
        return contacts.value;
    }
    const searchTerm = searchQuery.value.toLowerCase();
    return contacts.value.filter(
        (contact) =>
            contact.first_name.toLowerCase().includes(searchTerm) ||
            contact.last_name.toLowerCase().includes(searchTerm)
    );
});

const useFetch = async (url) => {
    const response = await fetch(url);
    return await response.json();
};

const fetchContacts = async () => {
    const data = await useFetch("http://localhost:3005/users");
    contacts.value = data;
};

fetchContacts();
</script>

<template>
    <div class="phone-book" dir="rtl">
        <header>
            <h1>دفترچه تلفن</h1>
            <div class="search-container">
                <input
                    v-model="searchQuery"
                    placeholder="جستجوی مخاطب..."
                    class="search-input"
                />
            </div>
        </header>
        <main>
            <table class="contact-table">
                <thead>
                    <tr>
                        <th>نام و نام خانوادگی</th>
                        <th>شماره تماس</th>
                        <th>موقعیت</th>
                        <th>تماس</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="contact in filteredContacts" class="contact-row">
                        <td>
                            {{ contact.first_name }} {{ contact.last_name }}
                        </td>
                        <td class="phone-number">{{ contact.username }}</td>
                        <td>{{ contact.city }}، {{ contact.province }}</td>
                        <td>
                            <button class="call-button">
                                <svg
                                    xmlns="http://www.w3.org/2000/svg"
                                    fill="none"
                                    viewBox="0 0 24 24"
                                    stroke-width="1.5"
                                    stroke="currentColor"
                                    class="size-6"
                                >
                                    <path
                                        stroke-linecap="round"
                                        stroke-linejoin="round"
                                        d="M20.25 3.75v4.5m0-4.5h-4.5m4.5 0-6 6m3 12c-8.284 0-15-6.716-15-15V4.5A2.25 2.25 0 0 1 4.5 2.25h1.372c.516 0 .966.351 1.091.852l1.106 4.423c.11.44-.054.902-.417 1.173l-1.293.97a1.062 1.062 0 0 0-.38 1.21 12.035 12.035 0 0 0 7.143 7.143c.441.162.928-.004 1.21-.38l.97-1.293a1.125 1.125 0 0 1 1.173-.417l4.423 1.106c.5.125.852.575.852 1.091V19.5a2.25 2.25 0 0 1-2.25 2.25h-2.25Z"
                                    />
                                </svg>
                            </button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </main>
    </div>
</template>

<style>
:root {
    --primary-color: #4a90e2;
    --secondary-color: #f0f4f8;
    --text-color: #333;
    --border-color: #e0e0e0;
    --hover-color: #e6f7ff;
    --button-color: #50b7f5;
}

body {
    font-family: "IRANSans", sans-serif;
    background-color: var(--secondary-color);
    color: var(--text-color);
    line-height: 1.6;
}

.phone-book {
    max-width: 900px;
    margin: 2rem auto;
    padding: 2rem;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

header {
    margin-bottom: 2rem;
}

h1 {
    font-size: 2rem;
    color: var(--primary-color);
    margin-bottom: 1rem;
}

.search-container {
    margin-bottom: 1.5rem;
}

.phone-number {
    direction: ltr;
    text-align: left;
}

.search-input {
    width: 100%;
    padding: 0.75rem 1rem;
    font-size: 1rem;
    border: 1px solid var(--border-color);
    border-radius: 4px;
    outline: none;
    transition: border-color 0.3s ease;
}

.search-input:focus {
    border-color: var(--primary-color);
}

.contact-table {
    width: 100%;
    border-collapse: collapse;
}

.contact-table th,
.contact-table td {
    padding: 1rem;
    text-align: right;
    border-bottom: 1px solid var(--border-color);
}

.contact-table th {
    background-color: var(--secondary-color);
    font-weight: bold;
    color: var(--primary-color);
}

.contact-row:hover {
    background-color: var(--hover-color);
}

.call-button {
    background-color: var(--button-color);
    color: white;
    border: none;
    width: 32px;
    height: 32px;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.call-button:hover {
    background-color: var(--primary-color);
}

@media (max-width: 768px) {
    .phone-book {
        padding: 1rem;
        margin: 1rem;
    }

    .contact-table {
        font-size: 0.9rem;
    }

    .contact-table th,
    .contact-table td {
        padding: 0.75rem 0.5rem;
    }
}
</style>
