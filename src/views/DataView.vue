<template>
  <div>
    <v-card-title>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="massive"
      :search="search"
      sort-by=""
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar flat>

          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
                New item
              </v-btn>
            </template>
              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="item.name"
                        label="Dessert name"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="item.year"
                        label="Year of birth"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="item.phone"
                        label="Phone"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="item.email"
                        label="Email"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">
                  Cancel
                </v-btn>
                <v-btn color="blue darken-1" text @click="save"> Save </v-btn>
              </v-card-actions>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="text-h2"
              >Do you want to delete this student?</v-card-title
              >
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete"
                >Cancel</v-btn
                >
                <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                >OK</v-btn
                >
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:[`item.actions`]="{ item }">
        <v-icon small class="mr-2" @click="editItem(item)"> mdi-account-edit </v-icon>
        <v-icon small @click="deleteItem(item)"> mdi-delete-forever </v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize"> Reset </v-btn>
      </template>
    </v-data-table>
  </div>
</template>

<script>

export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    search: '',
    headers: [
      { text: 'ПІБ', value: 'name', sortable: true},
      { text: 'Дата ', value: 'year', sortable: true },
      { text: 'Номер телефону', value: 'phone', sortable: true },
      { text: 'Електронна пошта', value: 'email', sortable: true },
      { text: 'Події', value: 'actions', sortable: false }
    ],
    massive: [],
    indexItem: -1,
    item: {
      name: '',
      year: 0,
      phone: '',
      email: ''
    },
    defaultItem: {
      name: '',
      year: 0,
      phone: '',
      email: ''
    }
  }),

  created () {
    this.initialize()
  },

  methods: {
    initialize () {
      this.massive = [
        {
          name: 'Антонюк Є.М.',
          year: 2000,
          phone: '+380964467616',
          email: 'student31@oa.edu.ua'
        },
        {
          name: 'Ваколюк В.В.',
          year: 2001,
          phone: '+380977073255',
          email: 'student20@oa.edu.ua'
        },
        {
          name: 'Корольчук А.М.',
          year: 2002,
          phone: '+380964467616',
          email: 'anna.korolchuk@oa.edu.ua'
        },
        {
          name: 'Костюшко О.А.',
          year: 2001,
          phone: '+380685710661',
          email: 'student45@oa.edu.ua'
        },
        {
          name: 'Кухта С.П.',
          year: 2001,
          phone: '+380977073255',
          email: 'student1@oa.edu.ua'
        },
        {
          name: 'Лобода А.О.',
          year: 2001,
          phone: '+380977073255',
          email: 'student2@oa.edu.ua'
        },
        {
          name: 'Луцюк М.І.',
          year: 2000,
          phone: '+380964467616',
          email: 'student3@oa.edu.ua'
        },
        {
          name: 'Мельник М.В.',
          year: 2002,
          phone: '+380964467616',
          email: 'student4@oa.edu.ua'
        },
        {
          name: 'Пляка С.П.',
          year: 2002,
          phone: '+380685710661',
          email: 'student5@oa.edu.ua'
        }
      ]
    },

    editItem (item) {
      this.indexItem = this.massive.indexOf(item)
      this.item = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem (item) {
      this.indexItem = this.massive.indexOf(item)
      this.item = Object.assign({}, item)
      this.dialogDelete = true
    },

    deleteItemConfirm () {
      this.massive.splice(this.indexItem, 1)
      this.closeDelete()
    },

    close () {
      this.dialog = false
      this.$nextTick(() => {
        this.item = Object.assign({}, this.defaultItem)
        this.indexItem = -1
      })
    },

    closeDelete () {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.item = Object.assign({}, this.defaultItem)
        this.indexItem = -1
      })
    },

    save () {
      if (this.indexItem > -1) {
        Object.assign(this.massive[this.indexItem], this.item)
      } else {
        this.massive.push(this.item)
      }
      this.close()
    }
  }
}
</script>
