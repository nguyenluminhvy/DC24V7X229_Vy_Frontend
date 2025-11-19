<script>
export default {
  name: 'ContactBook',
  data() {
    return {
      contacts: [],
      activeIndex: -1,
      searchText: '',
    }
  },
  watch: {
    searchText() {
      this.activeIndex = -1
    },
  },
  conputed: {
    contactStrings() {
      return this.contacts.map((contact) => {
        const { name, email, address, phone } = contact
        return [name, email, address, phone].join('')
      })
    },
    filteredContacts() {
      if (!this.searchText) return this.contacts
      return this.contacts.filter((_contact, index) =>
        this.contactStrings[index].includes(this.searchText),
      )
    },
    activeContact() {
      if (this.activeIndex < 0) return null
      return this.filteredContacts[this.activeIndex]
    },
    filteredContactsCount() {
      return this.filteredContacts.length
    },
  },
  methods: {
    async retrieveContacts() {
      try {
        this.contacts = await ContactService.getAll()
      } catch (error) {
        console.log(error)
      }
    },
    refreshList() {
      this.retrieveContacts()
      this.activeIndex = -1
    },
    async removeAllContacts() {
      if (confirm('Bạn muốn xóa tất cả Liên hệ?')) {
        try {
          await ContactService.deleteAll()
          this.refreshList()
        } catch (error) {
          console.log(error)
        }
      }
    },
    goToAddContact() {
      this.$router.push({ name: 'contact.add' })
    },
  },
  mounted() {
    this.refreshList()
  },
}
</script>
