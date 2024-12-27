<template>
    <div :class="currentView === 'welcome' ? 'h-screen relative bg-cover bg-center' : 'h-screen relative'" :style="currentView === 'welcome' ? { backgroundImage: `url(http://localhost:3000/uploads/background-image1.jpg)` } : {}">
        <!-- Strona powitalna -->
        <div v-if="currentView === 'welcome'" class="relative flex flex-col items-center justify-center h-full text-white px-4 bg-black bg-opacity-50">
            <button @click="currentView = 'login'" class="absolute top-4 right-4 bg-indigo-600 hover:bg-indigo-700 text-white py-2 px-6 rounded-full shadow-lg text-lg font-semibold transition duration-300">Logowanie/Rejestracja</button>
            <div class="w-full max-w-6xl">
                <div class="bg-gradient-to-r from-indigo-500 to-blue-500 text-white p-8 rounded-3xl shadow-2xl border-4 border-indigo-600">
                    <h2 class="text-4xl font-semibold text-center mb-4">Najtańsza wypożyczalnia samochodów na 1 dzień w Twoim mieście!</h2>
                    <p class="text-lg text-center">Z nami masz pewność, że płacisz najniższą cenę na rynku. Jeśli znajdziesz tańszą ofertę, zwrócimy Ci różnicę – to nasza gwarancja!</p>
                </div>
            </div>
        </div>

            <!-- Logowanie -->
    <div v-if="currentView === 'login'" class="h-screen relative bg-cover bg-center" :style="currentView === 'login' ? { backgroundImage: `url(http://localhost:3000/uploads/background-image1.jpg)` } : {}">
        <div class="absolute inset-0 flex items-center justify-center bg-black bg-opacity-50">
            <div class="bg-white text-gray-900 p-8 rounded-3xl shadow-xl w-full max-w-md">
                <h2 class="text-3xl font-semibold text-center mb-6">Logowanie</h2>
                <form @submit.prevent="loginUser">
                    <input v-model="loginData.email" placeholder="Email" class="border p-3 mb-4 w-full rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500" type="email" />
                    <input v-model="loginData.password" placeholder="Hasło" class="border p-3 mb-6 w-full rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500" type="password" />
                    <button type="submit" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white py-3 px-6 rounded-lg font-semibold shadow-lg transition duration-300">Zaloguj się</button>
                    <button @click="currentView = 'register'" class="w-full mt-4 bg-transparent border-2 border-indigo-600 text-indigo-600 hover:bg-indigo-600 hover:text-white py-3 px-6 rounded-lg font-semibold shadow-lg transition duration-300">Zarejestruj się</button>
                </form>
            </div>
        </div>
    </div>



        <!-- Rejestracja -->
        <div v-else-if="currentView === 'register'" class="h-screen relative bg-cover bg-center" :style="currentView === 'register' ? { backgroundImage: `url(http://localhost:3000/uploads/background-image1.jpg)` } : {}">
            <div class="absolute inset-0 flex items-center justify-center bg-black bg-opacity-50">
        <div class="bg-white text-gray-900 p-8 rounded-3xl shadow-xl w-full max-w-md">
            <h2 class="text-3xl font-semibold text-center mb-6">Rejestracja</h2>
            <form @submit.prevent="registerUser">
                <input v-model="registerData.email" placeholder="Email" class="border p-3 mb-4 w-full rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500" type="email" />
                <input v-model="registerData.password" placeholder="Hasło" class="border p-3 mb-6 w-full rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500" type="password" />
                <button type="submit" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white py-3 px-6 rounded-lg font-semibold shadow-lg transition duration-300">Zarejestruj</button>
                <button @click="currentView = 'login'" class="w-full mt-4 bg-transparent border-2 border-indigo-600 text-indigo-600 hover:bg-indigo-600 hover:text-white py-3 px-6 rounded-lg font-semibold shadow-lg transition duration-300">Powrót do logowania</button>
            </form>
        </div>
    </div>
</div>

            <!-- Widok użytkownika -->
    <div v-else-if="currentView === 'user'" class="h-screen w-full bg-gray-50">
    <div class="max-w-7xl mx-auto text-center mb-8 pt-12">
        <h1 class="text-4xl font-extrabold text-gray-900 mb-4">Dostępne samochody</h1>
        <button @click="logout" class="bg-teal-500 hover:bg-teal-600 text-white font-semibold py-3 px-8 rounded-2xl shadow-lg transform transition duration-300 hover:scale-105">Wyloguj</button>
    </div>


        <!-- Sekcja filtrowania i sortowania -->
        <div class="px-8 py-10 max-w-7xl mx-auto bg-white rounded-xl shadow-lg mb-12">
    <h3 class="text-3xl font-semibold text-gray-900 mb-6">Filtruj i sortuj samochody</h3>
    
    <!-- Filtry -->
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
        <div class="bg-white p-6 rounded-xl shadow-lg">
            <label class="block text-gray-900 font-semibold mb-2">Marka</label>
            <select v-model="filters.brand" class="border p-4 rounded-xl shadow-sm focus:outline-none focus:ring-2 focus:ring-teal-500 transition duration-200">
                <option value="">Wybierz markę</option>
                <option v-for="brand in availableBrands" :key="brand" :value="brand">{{ brand }}</option>
            </select>
        </div>

        <div class="bg-white p-6 rounded-xl shadow-lg">
            <label class="block text-gray-900 font-semibold mb-2">Model</label>
            <select v-model="filters.model" class="border p-4 rounded-xl shadow-sm focus:outline-none focus:ring-2 focus:ring-teal-500 transition duration-200">
                <option value="">Wybierz model</option>
                <option v-for="model in availableModels" :key="model" :value="model">{{ model }}</option>
            </select>
        </div>

        <div class="bg-white p-6 rounded-xl shadow-lg">
            <label class="block text-gray-900 font-semibold mb-2">Rok produkcji</label>
            <select v-model="filters.year" class="border p-4 rounded-xl shadow-sm focus:outline-none focus:ring-2 focus:ring-teal-500 transition duration-200">
                <option value="">Wszystkie lata</option>
                <option v-for="year in availableYears" :key="year" :value="year">{{ year }}</option>
            </select>
        </div>
    </div>

            <!-- Sekcja sortowania -->
<div class="mt-8">
    <label class="block text-xl font-semibold text-gray-900 mb-4">Sortuj według:</label>
    <div class="flex gap-6">
        <button @click="sortCars('year-asc')" class="bg-teal-500 text-white py-3 px-6 rounded-2xl hover:bg-teal-600 transform transition-all duration-300">Rok (rosnąco)</button>
        <button @click="sortCars('year-desc')" class="bg-teal-500 text-white py-3 px-6 rounded-2xl hover:bg-teal-600 transform transition-all duration-300">Rok (malejąco)</button>
        <button @click="sortCars('horsepower-asc')" class="bg-teal-500 text-white py-3 px-6 rounded-2xl hover:bg-teal-600 transform transition-all duration-300">Moc (rosnąco)</button>
        <button @click="sortCars('horsepower-desc')" class="bg-teal-500 text-white py-3 px-6 rounded-2xl hover:bg-teal-600 transform transition-all duration-300">Moc (malejąco)</button>
    </div>
</div>
</div>

        <!-- Lista samochodów -->
<div v-for="car in filteredCars" :key="car._id" class="mb-8 p-6 bg-white rounded-2xl shadow-md hover:shadow-xl transition-all duration-300 transform hover:scale-102">
    <div class="flex items-center justify-between mb-4">
        <div class="flex items-center space-x-6">
            <img :src="`http://localhost:3000${car.imagePath}`"
                alt="Zdjęcie auta"
                class="w-40 h-40 object-cover rounded-xl shadow-sm" />
            <div>
                <p class="text-2xl font-semibold text-gray-900">{{ car.brand }} {{ car.model }} ({{ car.year }}, {{ car.horsepower }} KM)</p>
            </div>
        </div>
        <div>
            <h3 class="font-semibold text-xl text-gray-900">Dostępne terminy:</h3>
            <ul v-if="car.availableDates.length" class="space-y-2 mt-2">
                <li v-for="date in car.availableDates" :key="date" class="flex justify-between items-center">
                    <span class="text-gray-600">{{ date }}</span>
                    <button @click="reserveCar(car._id, date)" class="bg-teal-600 hover:bg-teal-700 text-white py-2 px-6 rounded-2xl text-lg transition duration-200">
                        Zarezerwuj
                    </button>
                </li>
            </ul>
            <p v-else class="text-red-500 font-bold mt-2">Samochód chwilowo niedostępny.</p>
        </div>
    </div>
</div>
</div>



        <!-- Panel admina -->
<div v-else-if="currentView === 'admin'" class="container mx-auto px-8 py-10 bg-white">
    <h1 class="text-4xl font-extrabold text-gray-900 mb-8">Panel Administracyjny</h1>
<button @click="logout" class="bg-teal-500 hover:bg-teal-600 text-white font-semibold py-3 px-8 rounded-2xl shadow-lg transform transition duration-300 hover:scale-105 mb-2">Wyloguj</button>
<button @click="currentView = 'reservations'" class="bg-teal-500 hover:bg-teal-600 text-white font-semibold py-3 px-8 rounded-2xl shadow-lg transform transition duration-300 hover:scale-105 mb-2">Zobacz rezerwacje</button>
<button @click="showAddCarModal = true" class="bg-teal-500 hover:bg-teal-600 text-white font-semibold py-3 px-8 rounded-2xl shadow-lg transform transition duration-300 hover:scale-105 mb-2">Dodaj Samochód</button>
<!-- Sekcja filtrowania i sortowania -->
<div class="mb-8 p-8 bg-white rounded-xl shadow-lg hover:shadow-2xl transition duration-300 ease-in-out">
    <h3 class="text-3xl font-bold text-gray-900 mb-7">Filtruj i sortuj samochody</h3>
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8">
        <input v-model="filters.brand"
               type="text"
               placeholder="Filtruj po marce"
               class="p-5 rounded-xl border-2 border-teal-400 focus:ring-4 focus:ring-teal-300 focus:outline-none transition duration-200 ease-in-out hover:bg-teal-50" />
        <input v-model="filters.model"
               type="text"
               placeholder="Filtruj po modelu"
               class="p-5 rounded-xl border-2 border-teal-400 focus:ring-4 focus:ring-teal-300 focus:outline-none transition duration-200 ease-in-out hover:bg-teal-50" />
        <select v-model="filters.year" class="p-5 rounded-xl border-2 border-teal-400 focus:ring-4 focus:ring-teal-300 focus:outline-none transition duration-200 ease-in-out hover:bg-teal-50">
            <option value="">Wszystkie lata</option>
            <option v-for="year in availableYears" :key="year" :value="year">
                {{ year }}
            </option>
        </select>
    </div>

                    
                    <div class="mt-6">
        <label class="block font-semibold text-2xl text-gray-900 mb-4">Sortuj według:</label>
        <select v-model="sortOrder" class="border p-4 rounded-lg w-full bg-white focus:outline-none focus:ring-4 focus:ring-teal-400 shadow-lg transition duration-300 ease-in-out hover:bg-teal-50">
            <option value="year-asc" class="text-lg">Rok (rosnąco)</option>
            <option value="year-desc" class="text-lg">Rok (malejąco)</option>
            <option value="horsepower-asc" class="text-lg">Moc (rosnąco)</option>
            <option value="horsepower-desc" class="text-lg">Moc (malejąco)</option>
        </select>
    </div>
    </div>


            <!-- Lista samochodów -->
<div v-for="car in filteredCars" :key="car._id" class="border p-4 my-4 rounded-lg shadow-lg bg-white hover:shadow-xl transition duration-300 ease-in-out">
    <p class="text-2xl font-bold">{{ car.brand }} {{ car.model }} ({{ car.year }}, {{ car.horsepower }} KM)</p>
    <img v-if="car.imagePath"
         :src="`http://localhost:3000${car.imagePath}`"
         alt="Zdjęcie auta"
         class="w-48 h-48 object-cover rounded-xl shadow-lg mb-4" />
    <div class="flex justify-start space-x-3">
        <button @click="startEdit(car)" class="bg-blue-600 text-white hover:bg-blue-700 px-4 py-2 rounded-lg shadow-md transition duration-200">Edytuj</button>
        <button @click="deleteCar(car._id)" class="bg-red-600 text-white hover:bg-red-700 px-4 py-2 rounded-lg shadow-md transition duration-200">Usuń</button>
    </div>
    <h3 class="text-xl font-semibold mt-4">Dostępne terminy:</h3>
    <ul v-if="car.availableDates.length" class="text-sm space-y-2">
        <li v-for="date in car.availableDates" :key="date" class="flex justify-between items-center">
            <span>{{ date }}</span>
            <button @click="removeDateFromCar(car._id, date)" class="bg-yellow-500 text-white hover:bg-yellow-600 px-3 py-1 rounded-full transition duration-150">Usuń</button>
        </li>
    </ul>
    <button @click="openAddDatePanel(car._id)" class="bg-teal-500 hover:bg-teal-600 text-white px-4 py-2 rounded-lg mt-4 w-full">Dodaj termin</button>
</div>
</div>



       <!-- Modal dodawania samochodu -->
<div v-if="showAddCarModal" class="fixed inset-0 flex items-center justify-center bg-gray-900 bg-opacity-50">
    <div class="bg-white p-6 rounded-lg shadow-xl w-1/3 md:w-1/2 lg:w-1/4">
        <h3 class="text-xl font-bold text-center mb-4">Dodaj Nowy Samochód</h3>
        <form @submit.prevent="addCar">
            <input v-model="newCar.brand" placeholder="Marka" class="border p-3 mb-4 w-full rounded-lg focus:ring-2 focus:ring-teal-500" />
            <input v-model="newCar.model" placeholder="Model" class="border p-3 mb-4 w-full rounded-lg focus:ring-2 focus:ring-teal-500" />
            <input v-model="newCar.year" placeholder="Rok" type="number" class="border p-3 mb-4 w-full rounded-lg focus:ring-2 focus:ring-teal-500" />
            <input v-model="newCar.horsepower" placeholder="KM" type="number" class="border p-3 mb-4 w-full rounded-lg focus:ring-2 focus:ring-teal-500" />
            <input type="file" ref="imageFile" class="border p-3 mb-4 w-full rounded-lg focus:ring-2 focus:ring-teal-500" />
            
            <div class="flex justify-between space-x-4">
                <button type="submit" class="bg-teal-500 text-white hover:bg-teal-600 px-6 py-3 rounded-lg transition duration-300 w-full">Dodaj</button>
                <button type="button" @click="showAddCarModal = false" class="bg-red-500 text-white hover:bg-red-600 px-6 py-3 rounded-lg transition duration-300 w-full">Anuluj</button>
            </div>
        </form>
    </div>
</div>

       <!-- Widok rezerwacji -->
<div v-else-if="currentView === 'reservations'" class="container bg-white p-8">
    <h1 class="text-3xl font-bold text-center mb-6">Lista Rezerwacji</h1>

    <!-- Przycisk Powrót -->
    <button @click="currentView = 'admin'" class="bg-teal-500 hover:bg-teal-600 text-white font-semibold py-3 px-8 rounded-2xl shadow-lg transform transition duration-300 hover:scale-105 mb-6">
        Powrót
    </button>

    <div v-for="reservation in reservations" :key="reservation._id" class="bg-white border p-4 mb-4 rounded-lg shadow-lg hover:shadow-xl transition-shadow duration-300">
        <div class="flex justify-between items-center mb-3">
            <div>
                <p v-if="reservation.carId" class="font-semibold text-lg">
                    Samochód: {{ reservation.carId.brand }} {{ reservation.carId.model }}
                </p>
                <p v-else class="text-gray-500">Brak danych o samochodzie</p>
            </div>
            <div>
                <button @click="deleteReservation(reservation._id)" class="bg-red-500 text-white hover:bg-red-600 px-4 py-2 rounded-lg transition duration-300">
                    Usuń
                </button>
            </div>
        </div>
        
        <p class="text-gray-700">Data rezerwacji: {{ reservation.date }}</p>
        <p v-if="reservation.userId" class="text-gray-700">Użytkownik: {{ reservation.userId.email }}</p>
    </div>
</div>



        <!-- Panel edycji samochodu -->
<div v-if="editingCar !== null" class="fixed inset-0 flex items-center justify-center bg-gray-900 bg-opacity-50">
            <div class="bg-white p-5 rounded shadow-lg">
                <h3 class="text-lg font-bold mb-3">Edytuj pojazd</h3>
                <form @submit.prevent="saveEdit">
                    <input v-model="editingCar.brand" placeholder="Marka" class="border p-2 mb-2 w-full" />
                    <input v-model="editingCar.model" placeholder="Model" class="border p-2 mb-2 w-full" />
                    <input v-model="editingCar.year" placeholder="Rok" type="number" class="border p-2 mb-2 w-full" />
                    <input v-model="editingCar.horsepower" placeholder="KM" type="number" class="border p-2 mb-2 w-full" />
                    <input type="file" ref="editImageFile" class="border p-2 mb-2 w-full" />
                    <button type="submit" class="btn btn-success w-full">Zapisz zmiany</button>
                    <button @click="cancelEdit" class="btn btn-danger w-full mt-2">Anuluj</button>
                </form>
            </div>
        </div>

        <!-- Panel edycji samochodu -->
<div v-if="editingCar !== null" class="fixed inset-0 flex items-center justify-center bg-gray-900 bg-opacity-50">
    <div class="bg-white p-6 rounded-lg shadow-lg w-1/2">
        <h3 class="text-2xl font-bold text-center mb-6">Edytuj Pojazd</h3>
        <form @submit.prevent="saveEdit">
            <div class="mb-4">
                <input v-model="editingCar.brand" placeholder="Marka" class="border border-gray-300 p-3 rounded-lg w-full focus:outline-none focus:ring-2 focus:ring-blue-500" />
            </div>
            <div class="mb-4">
                <input v-model="editingCar.model" placeholder="Model" class="border border-gray-300 p-3 rounded-lg w-full focus:outline-none focus:ring-2 focus:ring-blue-500" />
            </div>
            <div class="mb-4">
                <input v-model="editingCar.year" placeholder="Rok" type="number" class="border border-gray-300 p-3 rounded-lg w-full focus:outline-none focus:ring-2 focus:ring-blue-500" />
            </div>
            <div class="mb-4">
                <input v-model="editingCar.horsepower" placeholder="KM" type="number" class="border border-gray-300 p-3 rounded-lg w-full focus:outline-none focus:ring-2 focus:ring-blue-500" />
            </div>
            <div class="mb-6">
                <input type="file" ref="editImageFile" class="border border-gray-300 p-3 rounded-lg w-full focus:outline-none focus:ring-2 focus:ring-blue-500" />
            </div>
            <div class="flex justify-between gap-3">
                <button type="submit" class="bg-green-500 text-white hover:bg-green-600 px-6 py-3 rounded-lg transition duration-300 w-1/2">
                    Zapisz zmiany
                </button>
                <button @click="cancelEdit" class="bg-red-500 text-white hover:bg-red-600 px-6 py-3 rounded-lg transition duration-300 w-1/2">
                    Anuluj
                </button>
            </div>
        </form>
    </div>
</div>


        <!-- Panel dodawania terminu -->
<div v-if="addingDateCarId !== null" class="fixed inset-0 flex items-center justify-center bg-gray-900 bg-opacity-50">
    <div class="bg-white p-6 rounded-lg shadow-lg w-1/3">
        <h3 class="text-2xl font-bold text-center mb-6">Dodaj Termin Wynajmu</h3>
        <input v-model="newDate" placeholder="RRRR-MM-DD" class="border border-gray-300 p-3 rounded-lg w-full focus:outline-none focus:ring-2 focus:ring-blue-500 mb-4" />
        <div class="flex justify-between gap-3">
            <button @click="addDateToCar" class="bg-green-500 text-white hover:bg-green-600 px-6 py-3 rounded-lg transition duration-300 w-1/2">
                Dodaj
            </button>
            <button @click="closeAddDatePanel" class="bg-red-500 text-white hover:bg-red-600 px-6 py-3 rounded-lg transition duration-300 w-1/2">
                Anuluj
            </button>
        </div>
    </div>
</div>
</div>
</template>

<<script>
import axios from "axios";

export default {
    data() {
    return {
        currentView: "welcome", // Widok początkowy
        loginData: { email: "", password: "" },
        registerData: { email: "", password: "" },
        cars: [], // Lista samochodów
        filteredCars: [], // Posortowana lista samochodów
        loggedInUserId: null,
        userRole: null,
        newCar: { brand: "", model: "", year: "", horsepower: "" },
        editingCar: null,
        newDate: "",
        addingDateCarId: null,
        reservations: [],
        modalMessage: null,
        showAddCarModal: false, // Modal dla dodawania samochodu
        filters: { brand: "", model: "", year: "" },
        sortOrder: "year-asc", // Domyślna opcja sortowania
    };
},
    computed: {
        filteredCars() {
            let cars = [...this.cars];

            // Filtrowanie
            if (this.filters.brand) {
                cars = cars.filter((car) =>
                    car.brand.toLowerCase().includes(this.filters.brand.toLowerCase())
                );
            }
            if (this.filters.model) {
                cars = cars.filter((car) =>
                    car.model.toLowerCase().includes(this.filters.model.toLowerCase())
                );
            }
            if (this.filters.year) {
                cars = cars.filter((car) => car.year.toString() === this.filters.year.toString());
            }

            // Sortowanie
            if (this.sortOrder === "year-asc") {
                cars.sort((a, b) => a.year - b.year);
            } else if (this.sortOrder === "year-desc") {
                cars.sort((a, b) => b.year - a.year);
            } else if (this.sortOrder === "horsepower-asc") {
                cars.sort((a, b) => a.horsepower - b.horsepower);
            } else if (this.sortOrder === "horsepower-desc") {
                cars.sort((a, b) => b.horsepower - a.horsepower);
            }

            return cars;
        },
        availableYears() {
            return [...new Set(this.cars.map((car) => car.year))].sort((a, b) => a - b);
        },
    },
    methods: {
        showModal(message) {
            this.modalMessage = message;
        },
        closeModal() {
            this.modalMessage = null;
        },
        applyFilters() {
            // Odświeżenie listy aut (computed automatycznie przelicza `filteredCars`)
        },
        validateEmail(email) {
            const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            return emailRegex.test(email);
        },
        validatePassword(password) {
            const passwordRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[^a-zA-Z\d\s]).{8,}$/;
            return passwordRegex.test(password);
        },
        validateHorsepower(horsepower) {
            const horsepowerRegex = /^\d+$/;
            return horsepowerRegex.test(horsepower);
        },
        validateYear(year) {
            const currentYear = new Date().getFullYear();
            return year >= 1886 && year <= currentYear; // Samochody od 1886 roku
        },
        validateDate(date) {
            const dateRegex = /^\d{4}-\d{2}-\d{2}$/;
            if (!dateRegex.test(date)) {
                return false;
            }
            const parsedDate = new Date(date);
            return parsedDate instanceof Date && !isNaN(parsedDate);
        },
        async registerUser() {
            try {
                if (!this.registerData.email || !this.registerData.password) {
                    this.showModal("Proszę wprowadzić email i hasło.");
                    return;
                }

                if (!this.validateEmail(this.registerData.email)) {
                    this.showModal("Podany adres email jest niepoprawny.");
                    return;
                }

                if (!this.validatePassword(this.registerData.password)) {
                    this.showModal(
                        "Hasło musi zawierać co najmniej 8 znaków, w tym jedną dużą literę, jedną małą literę, jedną cyfrę i jeden znak specjalny."
                    );
                    return;
                }

                const response = await axios.post("http://localhost:3000/register", this.registerData);
                this.showModal(response.data.message);
                this.currentView = "login";
            } catch (error) {
                this.showModal(error.response?.data?.message || "Błąd podczas rejestracji.");
            }
        },
        async loginUser() {
            try {
                if (!this.loginData.email || !this.loginData.password) {
                    this.showModal("Proszę wprowadzić email i hasło.");
                    return;
                }
                const response = await axios.post("http://localhost:3000/login", this.loginData);
                this.loggedInUserId = response.data.userId;
                this.userRole = response.data.role;
                this.currentView = this.userRole === "admin" ? "admin" : "user";
                this.fetchReservations();
                this.fetchCars();
            } catch (error) {
                this.showModal("Błąd podczas logowania.");
            }
        },
        async fetchCars() {
            try {
                const response = await axios.get("http://localhost:3000/cars");
                this.cars = response.data;
            } catch (error) {
                this.showModal("Błąd podczas pobierania listy samochodów.");
            }
        },
        async reserveCar(carId, date) {
            try {
                const response = await axios.post(`http://localhost:3000/cars/${carId}/reserve`, {
                    date,
                    userId: this.loggedInUserId,
                });

                if (response.data.activeReservation) {
                    const { car, date } = response.data.activeReservation;
                    this.showModal(
                        `Masz już aktywną rezerwację: ${car.brand} ${car.model} na dzień ${date}.`
                    );
                } else {
                    this.showModal("Rezerwacja zakończona sukcesem.");
                    this.fetchCars();
                }
            } catch (error) {
                this.showModal(error.response?.data?.message || "Błąd podczas rezerwacji.");
            }
        },
        async fetchReservations() {
            try {
                const response = await axios.get("http://localhost:3000/reservations");
                this.reservations = response.data;
            } catch (error) {
                this.showModal("Błąd podczas pobierania rezerwacji.");
            }
        },
        async deleteReservation(reservationId) {
            try {
                await axios.delete(`http://localhost:3000/reservations/${reservationId}`);
                this.showModal("Rezerwacja została usunięta.");
                this.fetchReservations();
            } catch (error) {
                this.showModal("Błąd podczas usuwania rezerwacji.");
            }
        },
        async addCar() {
            try {
                if (!this.validateHorsepower(this.newCar.horsepower)) {
                    this.showModal("Pole 'KM' musi zawierać tylko liczby.");
                    return;
                }
                if (!this.validateYear(this.newCar.year)) {
                    this.showModal("Pole 'Rok' musi zawierać poprawny rok.");
                    return;
                }

                const formData = new FormData();
                formData.append("brand", this.newCar.brand);
                formData.append("model", this.newCar.model);
                formData.append("year", this.newCar.year);
                formData.append("horsepower", this.newCar.horsepower);
                if (this.$refs.imageFile.files[0]) {
                    formData.append("image", this.$refs.imageFile.files[0]);
                }
                await axios.post("http://localhost:3000/cars", formData);
                this.showModal("Samochód został dodany.");
                this.fetchCars();
                this.showAddCarModal = false;
            } catch (error) {
                this.showModal("Błąd podczas dodawania samochodu.");
            }
        },
        async deleteCar(carId) {
            try {
                await axios.delete(`http://localhost:3000/cars/${carId}`);
                this.showModal("Samochód został usunięty.");
                this.fetchCars();
            } catch (error) {
                this.showModal("Błąd podczas usuwania samochodu.");
            }
        },
        async saveEdit() {
            try {
                if (!this.validateHorsepower(this.editingCar.horsepower)) {
                    this.showModal("Pole 'KM' musi zawierać tylko liczby.");
                    return;
                }
                if (!this.validateYear(this.editingCar.year)) {
                    this.showModal("Pole 'Rok' musi zawierać poprawny rok.");
                    return;
                }

                const formData = new FormData();
                formData.append("brand", this.editingCar.brand);
                formData.append("model", this.editingCar.model);
                formData.append("year", this.editingCar.year);
                formData.append("horsepower", this.editingCar.horsepower);
                if (this.$refs.editImageFile && this.$refs.editImageFile.files[0]) {
                    formData.append("image", this.$refs.editImageFile.files[0]);
                }
                await axios.put(`http://localhost:3000/cars/${this.editingCar._id}`, formData);
                this.showModal("Samochód został zaktualizowany.");
                this.fetchCars();
                this.editingCar = null;
            } catch (error) {
                this.showModal("Błąd podczas edycji samochodu.");
            }
        },
        async addDateToCar() {
            if (!this.validateDate(this.newDate)) {
                this.showModal("Proszę wprowadzić poprawną datę w formacie RRRR-MM-DD.");
                return;
            }
            try {
                await axios.post(`http://localhost:3000/cars/${this.addingDateCarId}/add-date`, {
                    date: this.newDate,
                });
                this.showModal("Termin dodany pomyślnie.");
                this.newDate = "";
                this.addingDateCarId = null;
                this.fetchCars();
            } catch (error) {
                this.showModal("Błąd podczas dodawania terminu.");
            }
        },
        async removeDateFromCar(carId, date) {
            try {
                await axios.post(`http://localhost:3000/cars/${carId}/remove-date`, { date });
                this.showModal("Termin został usunięty.");
                this.fetchCars();
            } catch (error) {
                this.showModal("Błąd podczas usuwania terminu.");
            }
        },
        startEdit(car) {
            this.editingCar = { ...car };
        },
        cancelEdit() {
            this.editingCar = null;
        },
        openAddDatePanel(carId) {
            this.addingDateCarId = carId;
        },
        closeAddDatePanel() {
            this.addingDateCarId = null;
            this.newDate = "";
        },
        logout() {
            this.currentView = "welcome";
            this.loggedInUserId = null;
            this.userRole = null;
        },
    },
    created() {
        this.fetchCars();
    },
};
</script>

