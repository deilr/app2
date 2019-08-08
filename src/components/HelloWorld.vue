<template>
    <div class="hello">
        <h1>{{ title }}</h1>
        <h3>Total notes: {{ notes.length }}</h3>

        <div class="form">
            <div class="form-group">
                <label>Note title</label>
                <input
                        class="form-control"
                        type="text"
                        v-model="note.title"
                        @keyup="chkInput('title')"
                        :class="isValid('title')"
                >
            </div>
            <div class="form-group">
                <label>Email</label>
                <input
                        class="form-control"
                        type="text"
                        v-model="note.email"
                        @keyup="chkInput('email')"
                        :class="isValid('email')"
                >
            </div>
            <div class="form-group">
                <label>Note text</label>
                <input
                        class="form-control"
                        type="text"
                        v-model="note.text"
                        @keyup="chkInput('text')"
                        :class="isValid('text')"
                >
            </div>
            <button
                    class="btn btn-primary mr-2"
                    @click="addNote"
                    :disabled="!isSubmitDisabled()"
            >Submit
            </button>
            <div class="btn-group">
                <button
                        type="button"
                        class="btn btn-primary"
                        :class="{ active: sortByDate }"
                        @click="sortNotes('date')"
                >По дате
                </button>
                <button
                        type="button"
                        class="btn btn-primary"
                        :class="{ active: sortByTitle }"
                        @click="sortNotes('title')"
                >По заголовку
                </button>
            </div>
        </div>

        <div class="col-sm-12 d-flex flex-wrap m-2">
            <div class="col-sm-4 note" v-for="(note, index) in notes">
                <div class="card">
                    <div class="card-block p-2">
                        <button class="close" @click="removeNote(index)">X</button>
                        <h4 class="card-title">{{ note.title }}</h4>
                        <h6 class="card-subtitle mb-3 text-muted">{{ note.email }}</h6>
                        <h6 class="card-subtitle mb-2 text-muted">{{
                            note.date.toLocaleDateString("ru", {
                            year: "numeric",
                            month: "2-digit",
                            day: "2-digit"
                            }
                            )
                            }}</h6>
                        <p class="card-text">{{note.text}}</p>
                        <button class="btn btn-primary btn-sm btn-block" @click="copy(index)">Дублировать</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'HelloWorld',
        // props: {
        //   msg: String
        // }
        data() {
            return {
                title: 'Notes',
                note: {
                    title: '',
                    email: '',
                    text: '',
                    // date: new Date(Date.now()).toLocaleString()
                },
                notes: [
                    {
                        title: '2Visited Hawaii',
                        text: 'I loved the beaches and delicious fresh coconuts',
                        email: 'a@d.lg.ua',
                        date: new Date(2020, 1, 2, 3, 4)
                    },
                    {
                        title: '1Visited London',
                        text: 'I loved the beaches and delicious fresh coconuts',
                        email: 'a@d.lg.ua',
                        date: new Date(2017, 1, 2, 5, 4)
                    },
                    {
                        title: '9Visited Hawaii',
                        text: 'I loved the beaches and delicious fresh coconuts',
                        email: 'a@d.lg.ua',
                        date: new Date(2017, 1, 2, 8, 4)
                    },
                    {
                        title: '8Visited London',
                        text: 'I loved the beaches and delicious fresh coconuts',
                        email: 'a@d.lg.ua',
                        date: new Date(2017, 1, 2, 9, 4)
                    }
                ],
                isSubmitDisabled() {
                    return this.titleIsValid && this.emailIsValid && this.textIsValid
                },
                titleReg: /^[A-ZА-Я].{2,}$/,
                emailReg: /^([a-z0-9_-]+\.)*[a-z0-9_-]+@[a-z0-9_-]+(\.[a-z0-9_-]+)*\.[a-z]{2,11}$/,
                textReg: /^(([\S]+)\s){2}([\S]+)/,
                titleIsValid: '',
                emailIsValid: '',
                textIsValid: '',
                sortByDate: true,
                sortByTitle: false,
                chkInput(targ) {
                    this[targ + 'IsValid'] = this[targ + 'Reg'].test(this.note[targ]);
                }
            }
        },
        computed: {},
        methods: {
            sortNotes(sortBy) {
                if (sortBy == 'date') {
                    this.sortByDate = true;
                    this.sortByTitle = false;
                    this.notes.sort(function (a, b) {
                        if (a.date > b.date) return 1;
                        if (a.date < b.date) return -1;
                        return 0;
                    })
                }
                if (sortBy == 'title') {
                    this.sortByDate = false;
                    this.sortByTitle = true;
                    this.notes.sort(function (a, b) {
                        if (a.title > b.title) return 1;
                        if (a.title < b.title) return -1;
                        return 0;
                    })
                }
            },
            removeNote(index) {
                this.notes.splice(index, 1)
            },
            copy(index) {
                let copyNote = {...this.notes[index]};
                copyNote.title += ' (copy)';
                copyNote.date = new Date(Date.now());
                this.notes.push(copyNote);
                this.sortNotes('date');
            },
            isValid(targ) {
                if (this[targ + 'IsValid'] === '') return '';
                return (this[targ + 'IsValid']) ? 'is-valid' : 'is-invalid'
            },
            addNote: function () {
                // this.notes.push({...this.note})
                let {text, email, title} = this.note;
                this.notes.push({
                    text,
                    email,
                    title,
                    date: new Date(Date.now())
                });
                this.note.title = '';
                this.note.email = '';
                this.note.text = '';
                //this.data.isSubmitDisabled = true
                this.titleIsValid = '';
                this.emailIsValid = '';
                this.textIsValid = '';
                this.sortNotes('date');
            }
        }
        ,
        mounted() {
            this.sortNotes('date')
        }
    }


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style type="text/css">

</style>
