new Vue({
  el: "#transactions",
  data: {
    items: [
      { title: "Feira", value: 105, positive: false },
      { title: "Salário", value: 1875, positive: true },
      { title: "Aluguel", value: 1200, positive: false },
      { title: "Conta de Luz", value: 150, positive: false },
      { title: "Conta de Água", value: 80, positive: false },
      { title: "Transporte", value: 200, positive: false },
      { title: "Venda de Produto", value: 300, positive: true },
      { title: "Jantar com Amigos", value: 120, positive: false },
      { title: "Seguro do Carro", value: 600, positive: false },
      { title: "Investimentos", value: 1500, positive: true },
      { title: "Compra de Roupas", value: 250, positive: false },
      { title: "Pagamento de Empréstimo", value: 350, positive: false },
    ],
    newTitle: '',
    newValue: 0,
    isPositive: true,
    hoveredIndex: null,
    currentPage: 0,
    itemsPerPage: 5
  },
  computed: {
    balance() {
      return this.items.reduce((acc, item) => {
        return acc + (item.positive ? item.value : -item.value);
      }, 0);
    },
    paginatedItems() {
      const start = this.currentPage * this.itemsPerPage;
      return this.items.slice(start, start + this.itemsPerPage);
    },
    totalPages() {
      return Math.ceil(this.items.length / this.itemsPerPage);
    }
  },
  methods: {
    add() {
      if (this.newTitle.trim() !== '' && !isNaN(this.newValue) && this.newValue !== 0) {
        const transaction = {
          title: this.newTitle,
          value: parseFloat(this.newValue),
          positive: this.isPositive
        };
        this.items.push(transaction);
        this.newTitle = '';
        this.newValue = 0;
        this.isPositive = true;
      } else {
        alert("Por favor, preencha todos os campos corretamente.");
      }
    },
    remove(index) {
      this.items.splice(index, 1);
    },
    moveUp(index) {
      const actualIndex = index + this.currentPage * this.itemsPerPage;
      if (actualIndex > 0) {
        const temp = this.items[actualIndex];
        this.items.splice(actualIndex, 1);
        this.items.splice(actualIndex - 1, 0, temp);
      }
    },
    moveDown(index) {
      const actualIndex = index + this.currentPage * this.itemsPerPage;
      if (actualIndex < this.items.length - 1) {
        const temp = this.items[actualIndex];
        this.items.splice(actualIndex, 1);
        this.items.splice(actualIndex + 1, 0, temp);
      }
    },
    calculateBalance(index) {
      return this.items.slice(0, index + 1).reduce((acc, item) => {
        return acc + (item.positive ? item.value : -item.value);
      }, 0);
    },
    nextPage() {
      if (this.currentPage < this.totalPages - 1) {
        this.currentPage++;
      }
    },
    prevPage() {
      if (this.currentPage > 0) {
        this.currentPage--;
      }
    }
  }
});
