<template>
    <div class="up_menu">
        <div>
            <div class="logo_container">
                <img src="/img/logo.png" alt=""/>
            </div>
            <div class="children_container"></div>
        </div>
        <div class="settings_container">
            <div class="settings_item burger" v-on:click="openMenuModal">
                <font-awesome-icon :icon="['fas', 'align-justify']"/>
            </div>
            <div class="settings_item user" v-on:click="openProfileModal">
                <img :src="img" alt="">
            </div>
        </div>
        <div class="modal_container" v-if="data.openModal">
            <div class="profile" v-if="data.modalType === 'user'">
                <div class="col-12 p-0 d-flex justify-content-center">
                    <div class="profile-img-container">
                        <img :src="img" alt="">
                    </div>
                </div>
                <div class="profile-name-container">{{user.name}} {{user.surname}}</div>
                <div class="profile-email-container">{{user.email}}</div>
                <div class="profile-button-container">
                    <div class="col-12 p-0 pb-2 d-flex justify-content-center">
                        <div class="profile-button">Редактировать</div>
                    </div>
                    <div class="col-12 p-0 pb-2 d-flex justify-content-center">
                        <div class="profile-button" v-on:click="logout">Выйти</div>
                    </div>
                </div>
            </div>
            <div class="menu" v-if="data.modalType === 'menu'">
                <div class="menu-item-wrapper">
                    <div class="menu-item" :title="'Аккаунт'" v-on:click="openProfile">
                        <div class="menu-item-icon-container">
                            <div class="img-wrapper">
                                <font-awesome-icon :icon="['fas', 'users']"/>
                            </div>
                        </div>
                        <div class="menu-item-description-container">
                            Аккаунт
                        </div>
                    </div>
                    <div class="menu-item" :title="'Расчет'" v-if="user.role_id === 2" v-on:click="openComputation">
                        <div class="menu-item-icon-container">
                            <div class="img-wrapper">
                                <font-awesome-icon :icon="['fas', 'calculator']"/>
                            </div>
                        </div>
                        <div class="menu-item-description-container">
                            Расчет КП
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="overflow" v-if="data.openModal" v-on:click="closeModal"></div>
    </div>
</template>

<script>
    import Vue from "vue";

    export default Vue.extend({
        computed: {
            root() {
                return this.$root;
            },
            user() {
                return this.root.data.user;
            },
            img() {
                let img = this.user.img;
                if (img) {
                    return img;
                } else {
                    return '/img/no_photo.png';
                }
            }
        },
        data: function () {
            return {
                data: {
                    openModal: false,
                    modalType: '',
                }
            }
        },
        methods: {
            logout() {
                let $this = this;
                let options = {
                    'url': '/logout',
                    'data': [],
                    'showError': null,
                    'success': (response) => {
                        $this.root.setUser({});
                    },
                    'error': () => {
                    },
                };
                $this.root.request(options);
            },
            openProfileModal() {
                this.data.openModal = true;
                this.data.modalType = 'user';
            },
            openMenuModal() {
                this.data.openModal = true;
                this.data.modalType = 'menu';
            },
            closeModal() {
                this.data.openModal = false;
                this.data.modalType = '';
            },
            openProfile() {
                location.href = 'https://accounts.isp.team/';
            },
            openComputation() {
                location.href = 'https://computation.isp.team/';
            }
        }
    });
</script>

<style lang="scss" scoped>

    .up_menu {
        display: inline-flex !important;
        justify-content: space-between;
        width: 100%;
        background-color: #2e333b !important;
        padding-top: 0.5rem;
        padding-bottom: 0.5rem;
        align-items: center;
        position: relative;

        .logo_container {
            margin-left: 0.5rem;
            margin-right: 0.5rem;

            img {
                width: 50px;
            }
        }

        .settings_container {
            display: flex;

            .settings_item {
                cursor: pointer;
                width: 35px;
                height: 35px;
                display: flex;
                align-items: center;
                justify-content: center;
                text-transform: uppercase;
                line-height: 1rem;
                color: rgba(209, 173, 103, 1);
                margin-left: 0.5rem;
                margin-right: 0.5rem;

                &.burger {
                    font-size: 2rem;
                }

                &.user {
                    /*border: 1px solid #d1ad67;*/
                    border-radius: 50%;
                    overflow: hidden;

                    img {
                        width: 100%;
                    }
                }

                &:hover {
                    opacity: 0.5;
                }
            }
        }

        .modal_container {
            position: absolute;
            width: 300px;
            background-color: white;
            right: 10px;
            top: 100%;
            border: 1px solid rgba(0, 0, 0, .2);
            z-index: 2000000;
            box-shadow: 0 2px 10px rgba(0, 0, 0, .2);

            .menu {
                .menu-item-wrapper {
                    width: 100%;
                    padding: 0.5rem;
                }

                .menu-item-wrapper .menu-item {
                    display: inline-flex;
                    width: 75px;
                    height: 75px;
                    flex-wrap: wrap;
                    align-items: center;
                    justify-content: center;
                    cursor: pointer;
                    flex-direction: column;
                    margin: 0.5rem;
                }

                .menu-item-wrapper .menu-item:hover {
                    background-color: rgba(158, 154, 154, 0.05);
                }

                .menu-item-wrapper .menu-item .menu-item-icon-container {
                    font-size: 2.5rem;
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    padding: 0.5rem;
                    width: 100%;
                    height: 65%;
                }

                .menu-item-wrapper .menu-item .menu-item-icon-container .img-wrapper {
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    color: #2e333b;
                }

                .menu-item-wrapper .menu-item .menu-item-description-container {
                    width: 100%;
                    padding-bottom: 0.25rem;
                    text-align: center;
                    font-size: 1rem;
                    text-overflow: ellipsis;
                    white-space: nowrap;
                    overflow: hidden;
                    padding-left: 0.25rem;
                    padding-right: 0.25rem;
                    height: 22%;
                    color: #2e333b;
                }
            }


            .profile {
                display: flex;
                flex-wrap: wrap;
                align-items: flex-start;
                justify-content: center;
                padding-top: 1rem;


                .profile-img-container {
                    width: 100px;
                    height: 100px;
                    overflow: hidden;
                    border: 1px solid #dce0e4;
                    justify-content: center;
                    align-items: flex-start;
                    display: flex;
                    border-radius: 50%;

                    img {
                        max-height: 125px;
                        max-width: 125px;
                    }
                }

                .profile-name-container {
                    width: 100%;
                    font-weight: bold;
                    margin-top: 1rem;
                    font-size: 1.2rem;
                    text-align: center;
                }

                .profile-button-container {
                    width: 100%;
                    padding-top: 1rem;
                    padding-left: 1rem;
                    padding-right: 1rem;
                    padding-bottom: 1rem;
                    display: flex;
                    align-items: center;
                    flex-wrap: wrap;
                    justify-content: center;

                    .profile-button {
                        border-radius: 3px;
                        border: 1px solid #dce0e4;
                        padding-top: 5px;
                        padding-bottom: 5px;
                        padding-left: 10px;
                        padding-right: 10px;
                        cursor: pointer;
                    }
                }

            }
        }

        .overflow {
            width: 100%;
            height: 100vh;
            position: absolute;
            left: 0;
            top: 0;
            z-index: 1999999;
        }
    }
</style>
