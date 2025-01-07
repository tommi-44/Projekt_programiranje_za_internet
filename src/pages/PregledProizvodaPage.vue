<template>
  <div class="q-pa-md">
    <!-- Prikaz tablice -->
    <q-table
      title="Proizvodi"
      :rows="rows"
      :columns="columns"
      row-key="id"
    />
    <!-- Debug - Prikaz grešaka ako postoje -->
    <div v-if="error" class="text-red">
      {{ error }}
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'; // Vue funkcionalnosti
import axios from 'axios'; // Axios za API poziv

export default {
  setup() {
    // Definicija stupaca tablice
    const columns = [
      { name: 'id', align: 'left', label: 'ID', field: 'id', sortable: true },
      { name: 'proizvod', align: 'left', label: 'Proizvod', field: 'proizvod', sortable: true },
      { name: 'materijal', align: 'left', label: 'Materijal', field: 'materijal', sortable: true },
      { name: 'dimenzije', align: 'left', label: 'Dimenzije', field: 'dimenzije', sortable: true },
      { name: 'kolicina_na_skladistu', align: 'left', label: 'Količina na skladištu', field: 'kolicina_na_skladistu', sortable: true },
      { name: 'cijena', align: 'left', label: 'Cijena', field: 'cijena', sortable: true },
      { name: 'prodavac', align: 'left', label: 'Prodavač', field: 'prodavac', sortable: true },
    ]

    // Reaktivne varijable
    const rows = ref([]); // Za podatke iz baze
    const error = ref(null); // Za greške
    const debug = ref(false); // Debug mod

    // Funkcija za dohvat podataka iz baze
    const loadProizvodi = async () => {
      try {
        const response = await axios.get('http://localhost:3000/api/proizvodi');
        console.log("API Response:", response);
        rows.value = response.data; // Spremanje podataka u reaktivnu varijablu
      } catch (err) {
        console.error("API Error:", err);
        error.value = `Greška API poziva: ${err.message}`; // Prikaz točne greške
      }
    };

    // Automatski poziv API-ja kada se komponenta učita
    onMounted(() => {
      debug.value = true; // Aktiviranje debug moda
      loadProizvodi();
    });

    // Vraćanje vrijednosti u template
    return { rows, columns, error, debug };
  },
};
</script>
