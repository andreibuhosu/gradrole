<template>
  <div>
    <h2 id="page-heading" data-cy="UserExtHeading">
      <span v-text="$t('gradroleApp.userExt.home.title')" id="user-ext-heading">User Exts</span>
      <div class="d-flex justify-content-end">
        <button class="btn btn-info mr-2" v-on:click="handleSyncList" :disabled="isFetching">
          <font-awesome-icon icon="sync" :spin="isFetching"></font-awesome-icon>
          <span v-text="$t('gradroleApp.userExt.home.refreshListLabel')">Refresh List</span>
        </button>
        <router-link
          :to="{ name: 'UserExtCreate' }"
          tag="button"
          id="jh-create-entity"
          data-cy="entityCreateButton"
          class="btn btn-primary jh-create-entity create-user-ext"
        >
          <font-awesome-icon icon="plus"></font-awesome-icon>
          <span v-text="$t('gradroleApp.userExt.home.createLabel')"> Create a new User Ext </span>
        </router-link>
      </div>
    </h2>
    <br />
    <div class="alert alert-warning" v-if="!isFetching && userExts && userExts.length === 0">
      <span v-text="$t('gradroleApp.userExt.home.notFound')">No userExts found</span>
    </div>
    <div class="table-responsive" v-if="userExts && userExts.length > 0">
      <table class="table table-striped" aria-describedby="userExts">
        <thead>
          <tr>
            <th scope="row"><span v-text="$t('global.field.id')">ID</span></th>
            <th scope="row"><span v-text="$t('gradroleApp.userExt.middleName')">Middle Name</span></th>
            <th scope="row"><span v-text="$t('gradroleApp.userExt.jobRole')">Job Role</span></th>
            <th scope="row"><span v-text="$t('gradroleApp.userExt.dateOfBirth')">Date Of Birth</span></th>
            <th scope="row"><span v-text="$t('gradroleApp.userExt.user')">User</span></th>
            <th scope="row"></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="userExt in userExts" :key="userExt.id" data-cy="entityTable">
            <td>
              <router-link :to="{ name: 'UserExtView', params: { userExtId: userExt.id } }">{{ userExt.id }}</router-link>
            </td>
            <td>{{ userExt.middleName }}</td>
            <td>{{ userExt.jobRole }}</td>
            <td>{{ userExt.dateOfBirth }}</td>
            <td>
              {{ userExt.user ? userExt.user.login : '' }}
            </td>
            <td class="text-right">
              <div class="btn-group">
                <router-link
                  :to="{ name: 'UserExtView', params: { userExtId: userExt.id } }"
                  tag="button"
                  class="btn btn-info btn-sm details"
                  data-cy="entityDetailsButton"
                >
                  <font-awesome-icon icon="eye"></font-awesome-icon>
                  <span class="d-none d-md-inline" v-text="$t('entity.action.view')">View</span>
                </router-link>
                <router-link
                  :to="{ name: 'UserExtEdit', params: { userExtId: userExt.id } }"
                  tag="button"
                  class="btn btn-primary btn-sm edit"
                  data-cy="entityEditButton"
                >
                  <font-awesome-icon icon="pencil-alt"></font-awesome-icon>
                  <span class="d-none d-md-inline" v-text="$t('entity.action.edit')">Edit</span>
                </router-link>
                <b-button
                  v-on:click="prepareRemove(userExt)"
                  variant="danger"
                  class="btn btn-sm"
                  data-cy="entityDeleteButton"
                  v-b-modal.removeEntity
                >
                  <font-awesome-icon icon="times"></font-awesome-icon>
                  <span class="d-none d-md-inline" v-text="$t('entity.action.delete')">Delete</span>
                </b-button>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <b-modal ref="removeEntity" id="removeEntity">
      <span slot="modal-title"
        ><span id="gradroleApp.userExt.delete.question" data-cy="userExtDeleteDialogHeading" v-text="$t('entity.delete.title')"
          >Confirm delete operation</span
        ></span
      >
      <div class="modal-body">
        <p id="jhi-delete-userExt-heading" v-text="$t('gradroleApp.userExt.delete.question', { id: removeId })">
          Are you sure you want to delete this User Ext?
        </p>
      </div>
      <div slot="modal-footer">
        <button type="button" class="btn btn-secondary" v-text="$t('entity.action.cancel')" v-on:click="closeDialog()">Cancel</button>
        <button
          type="button"
          class="btn btn-primary"
          id="jhi-confirm-delete-userExt"
          data-cy="entityConfirmDeleteButton"
          v-text="$t('entity.action.delete')"
          v-on:click="removeUserExt()"
        >
          Delete
        </button>
      </div>
    </b-modal>
  </div>
</template>

<script lang="ts" src="./user-ext.component.ts"></script>
