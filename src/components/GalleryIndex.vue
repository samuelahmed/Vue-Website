<template>
  <div class="row">
    <div class="row-12">
        <div class="q-mt-xl text-center">
          <p class="text-h4">Attributes</p>
          <q-btn class="q-ma-sm" style="background-color:white" @click="resetAllFilters">Reset all Filters</q-btn>
        </div>
      <div class="q-pt-none text-center" style="width: 17vw; min-width: 150px;">
        <div class="q-pt-sm">
          <q-select
            hide-dropdown-icon
            filled
            v-model="modelEdition"
            use-input
            use-chips
            multiple
            max-values="1"
            input-debounce="0"
            @new-value="createValue"
            @filter="filterFn"
            label="Search by ID">
            <template v-slot:append>
              <q-icon name="search" @click.stop />
            </template>
          </q-select>
        </div> 
        <div class="q-pt-sm">
          <q-select
            v-model="modelHead"
            multiple
            bg-color="white"
            outlined
            use-input 
            :options="optionsHead"
            use-chips
            max-values="1"
            label="Head"
          />
        </div>
        <div class="q-pt-sm">
          <q-select
            v-model="modelJewelry"
            multiple
            bg-color="white"
            outlined
            use-input 
            :options="optionsJewelry"
            use-chips
            max-values="1"
            label="Jewelry"
          />
        </div>
        <div class="q-pt-sm">
          <q-select
            v-model="modelGlasses"
            multiple
            bg-color="white"
            outlined
            use-input 
            :options="optionsGlasses"
            use-chips
            max-values="1"
            label="Glasses"
          />
        </div>
        <div class="q-pt-sm">
          <q-select
            v-model="modelEyes"
            multiple
            bg-color="white"
            outlined
            use-input 
            :options="optionsEyes"
            use-chips
            max-values="1"
            label="Eyes"
          />
        </div>      
        <div class="q-pt-sm">
          <q-select
            v-model="modelFace"
            multiple
            bg-color="white"
            outlined
            use-input
            :options="optionsFace"
            use-chips
            max-values="1"
            label="Face"
          />
        </div>
        <div class="q-pt-sm">
          <q-select
            v-model="modelLips"
            multiple
            bg-color="white"
            outlined
            use-input 
            :options="optionsLips"
            use-chips
            max-values="1"
            label="Lips"
          />
        </div>
        <div class="q-pt-sm">
          <q-select
            v-model="modelSkin"
            multiple
            bg-color="white"
            outlined
            use-input 
            :options="optionsSkin"
            use-chips
            max-values="1"
            label="Skin"
          />
        </div>
        <div class="q-pt-sm">
          <q-select
            v-model="modelSun"
            multiple
            outlined
            use-input 
            bg-color="white"
            :options="optionsSun"
            use-chips
            max-values="1"
            label="Sun"
            />
        </div>
        <div class="q-pt-sm">
          <q-select
            v-model="modelBackground"
            multiple
            use-input
            map-options
            emit-value
            option-value="id"
            option-label="name"
            outlined
            bg-color="white"
            :options="optionsBackground"
            use-chips
            max-values="1"
            label="Background"
          />
        </div>
      </div>
    </div>
    <div class="col">
      <div class="q-ma-md">
        <q-scroll-area class="q-mt-xl" style="height: 800px; width: 60vw; min-width: 200px">
          <div class="q-py-xs">
            <q-page class="flex flex-center">
              <div class="q-pa-md">
                <q-table
                  grid
                  :rows="items"
                  row-key="name"
                  table-style="overflow-y:hidden"
                  :filter="buildFilter()"
                  :filter-method="filterProducts"
                  hide-header
                  :columns="columns"
                  virtual-scroll
                  :pagination="{rowsPerPage: 150}"
                  :rows-per-page-options="[50, 100, 150, 250, 500, 1000]"
                  >
                  <template v-slot:item="props">
                    <q-card-section class="q-pa-none row flex flex-center" style="width: 79px; height: 79px">
                      <TableImgAncient
                        @click="navToAncientDetails(props.row.edition)"
                        :edition="props.row.edition"
                        :attributes="props.row.attributes"
                        >
                      </TableImgAncient>
                    </q-card-section>
                  </template>
                </q-table>
              </div>
            </q-page>
          </div>
        </q-scroll-area>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue'
import TableImgAncient from './TableImgAncient.vue'
import { ref } from 'vue'

const stringOptions = []
const filterOptions = ref(stringOptions)
const columns = [
  {
    name: 'edition',
    label: 'Edition',
    sortable: true,
    required: true,
    sort: true,
    filter: true,
    field: row => row.edition,
    format: val => `${val}`,
    filterMethod: (val, row) => row.edition.toString().includes(val)
  },
  {
    name: 'attributes',
    label: 'Attributes',
    sortable: true,
    required: true,
    filter: true,
    field: row => row.attributes.map( x => x.trait_type + ' ' + x.value).join( ' ' ),
    format: val => `${val}`,
    },
    {
    name: 'name',
    label: 'Name',
    sortable: true,
    required: true,
    filter: true,
    field: row => row.name,
    format: val => `${val}`,
  },
]

export default defineComponent({
  name: 'GalleryIndex',
  components: {
    TableImgAncient,
  },
setup () {
    return {
      modelBackground: ref([]),
      modelSun: ref([]),
      modelSkin: ref([]),
      modelEyes: ref([]),
      modelLips: ref([]),
      modelFace: ref([]),
      modelGlasses: ref([]),
      modelJewelry: ref([]),
      modelHead: ref([]),
      modelEdition: ref([]),
      filterOptions,
      needle: ref(''),
      model: ref(null),

      optionsBackground: [
        'Daylight', 'Forest', 'Metaverse', 'Martian', 'Nighttime', 'Ocean'
      ],
      optionsSun: [
        'Golden', 'Metaverse Blue', 'Purple', 'Pink', 'Red', 'Standard'
      ],
      optionsSkin: [
        'Human Skin A', 'Human Skin B', 'Human Skin C', 'Human Skin D', 'Human Skin E', 'Human Skin F', 'Human Skin G', 'Human Skin H', 'Human Skin I', 'Human Skin J', 'Human Skin K', 'Human Skin L', 'Human Skin M'
      ],
      optionsEyes: [
        'Blood Tears', 'Bloodshot', 'Blue', 'Brown', 'Cross', 'Golden', 'Green', 'Nature Tears', 'Mauve Tears', 'Pink', 'Purple', 'Red', 'Standard', 'Swirly Eye Tattoo', 'Teal Tears', 'White'  
      ],
      optionsLips: [
        'Admiral', 'Beige', 'Brown', 'Garnet', 'Green', 'Maya', 'Metaverse Blue', 'Orange', 'Purple', 'Red', 'Royal', 'Teal'
      ],
      optionsFace: [
        'Blue Circle', 'Blue Heart Cheek', 'Curly Mustache', 'Green Handlebars', 'Grey Beard', 'Heaven Sign', 'Mask', 'Pink Circle', 'Pink Handlebars', 'Pink Heart Cheek', 'Seven Tattoo', 'Standard', 'Stubble', 'Yellow Circle', 'Yellow Heart Cheek', 'Young Beard'
      ],
      optionsGlasses: [
        'Active Red', 'Eye Patch', 'Fashion', 'None', 'Purple', 'Seeing', 'Sunglasses', 
      ],
      optionsJewelry: [
        'Face Chain', 'Gold Chain', 'Gold Cross', 'Golden Round Earring', 'Green Neck Swag', 'Metaverse Blue Gold', 'Multiple Gold Earrings', 'None', 'Pearls', 'Purple Gem', 'Purple Neck Swag', 'Red Neck Swag', 'Scarab Cross', 'Yellow Neck Swag' 
      ],
      optionsHead: [
        'Ancient Queen', 'Ancient Ribbon', 'Basic Priest', 'Blue Hat', 'Brown Punk Hair', 'Crazy Blue Hair', 'Crazy Green Hair', 'Crazy Grey Hair', 'Gold Chapeau', 'Grey Hoodie', 'Grey Punk Hair', 'Hardhat', 'Lego Brown', 'Lego Orange', 'Long Blue Hair', 'Long Purple Hair', 'Metapriest', 'Metaverse Blue Punk Hair', 'Metaverse Queen', 
        'Multicolor Hat', 'Napoleon Hat', 'NPC Brown', 'Old Grey Hair', 'Pink Killa', 'Purple Blue Bangs', 'Purple Hoodie', 'Side Ponytail', 'Standard', 'Underground Pink'
      ],
      createValue (val, done) {
        if (val.length > 0) {
          if (!stringOptions.includes(val)) {
            stringOptions.push(val)
          }
          done(val, 'toggle')
        }
      },
      filterFn (val, update) {
        update(() => {
          if (val === '') {
            filterOptions.value = stringOptions
          }
          else {
            const needle = val.toLowerCase()
            filterOptions.value = stringOptions.filter(
              v => v.toLowerCase().indexOf(needle) > -1
            )
          }
        })
      }
    }
  }, 
  data: () => ({
    items: [],
    filter: '',
    terms: '',
    columns,
    rows: [],
    Selected: [],
    list: [],
  }),
  async created () {
    try {
      const res = await this.$api.get('/json/metadata.json')
      res.status === 200
        ? (this.items = res.data)
        : alert('there was an error getting items here')
    } catch (error) {
      alert('there was an error getting items at this location')
    }
  },
  computed: {
    filterProducts() {
      if (this.modelEdition > 0)
        return this.numberSearchFilter 
      else
        return this.myFilter
    }
  },
  methods: {
    navToAncientDetails (edition) {
      this.$router.push({ name: 'AncientDetails', params: { edition } })
    },
    buildFilter() {
      //STILL BROKEN IF VALUE IS SELECTED THAT HAS MORE THAN ONE PAGE
      if (
        this.modelBackground.length > 0 ||
        this.modelSun.length > 0 ||
        this.modelSkin.length > 0 ||
        this.modelEyes.length > 0 ||
        this.modelLips.length > 0 ||
        this.modelFace.length > 0 ||
        this.modelGlasses.length > 0 ||
        this.modelJewelry.length > 0 ||
        this.modelHead.length > 0 ||
        this.modelEdition.length > 0
        ) 
      return  [ 
        this.modelBackground,
        this.modelSun,
        [],//this._ignore
        this.modelSkin,
        this.modelEyes,
        this.modelLips,
        this.modelFace,
        this.modelGlasses,
        this.modelJewelry,
        this.modelHead,
        this.modelEdition
      ];
    },
    myFilter (rows, filterValues) {
      return rows.filter(
        row => { 
          for (let i = 0; i <= 9; i += 1){ 
            if (filterValues[i].length && 
              !filterValues[i].some(filterValue => (row.attributes[i]).value.toLowerCase().includes(filterValue.toLowerCase())))
              return false;
          }
          return true;
        }
      )
    },
    numberSearchFilter(rows, terms, cols, cellValue) {
      const lowerTerms = filterOptions.value[filterOptions.value.length - 1]
      return rows.filter(
        row => cols.some(col => (cellValue(col, row)).toLowerCase() === lowerTerms))
    },
    resetAllFilters() {
      this.modelBackground = []
      this.modelSun = []
      this.modelSkin = []
      this.modelEyes = []
      this.modelLips = []
      this.modelFace = []
      this.modelGlasses = []
      this.modelJewelry = []
      this.modelHead = []
      this.modelEdition = []
    },
  }
})
</script>


