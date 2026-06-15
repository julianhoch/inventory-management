<template>
  <div class="app" :class="{ 'sidebar-collapsed': isSidebarCollapsed }">
    <!-- Mobile overlay -->
    <div
      v-if="isSidebarOpen"
      class="sidebar-overlay"
      @click="isSidebarOpen = false"
    ></div>

    <!-- Sidebar -->
    <aside class="sidebar" :class="{ 'is-open': isSidebarOpen, 'is-collapsed': isSidebarCollapsed }">
      <div class="sidebar-logo">
        <span class="sidebar-logo-icon">
          <!-- factory/cube SVG -->
          <svg width="28" height="28" viewBox="0 0 28 28" fill="none">
            <rect width="28" height="28" rx="6" fill="#2563eb"/>
            <path d="M6 20V12L11 9V12L16 9V20H6Z" fill="white" opacity="0.9"/>
            <path d="M16 20V9L22 12V20H16Z" fill="white" opacity="0.6"/>
          </svg>
        </span>
        <div class="sidebar-logo-text-block">
          <span class="sidebar-logo-name">{{ t('nav.companyName') }}</span>
          <span class="sidebar-logo-sub">{{ t('nav.subtitle') }}</span>
        </div>
        <button
          class="sidebar-collapse-btn"
          @click="toggleSidebarCollapse"
          :aria-label="isSidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'"
          :title="isSidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'"
        >
          <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
            <path d="M10 3L5 8L10 13" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
      </div>

      <nav class="sidebar-nav">
        <router-link
          to="/"
          class="sidebar-link"
          :class="{ 'router-link-active': $route.path === '/' }"
          title="Dashboard"
          @click="isSidebarOpen = false"
        >
          <svg class="sidebar-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <rect x="2" y="2" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <rect x="10" y="2" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <rect x="2" y="10" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <rect x="10" y="10" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span class="sidebar-label">{{ t('nav.overview') }}</span>
        </router-link>

        <router-link
          to="/inventory"
          class="sidebar-link"
          title="Inventory"
          @click="isSidebarOpen = false"
        >
          <svg class="sidebar-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <path d="M9 2L16 6V12L9 16L2 12V6L9 2Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            <path d="M9 2V16M2 6L9 10L16 6" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span class="sidebar-label">{{ t('nav.inventory') }}</span>
        </router-link>

        <router-link
          to="/orders"
          class="sidebar-link"
          title="Orders"
          @click="isSidebarOpen = false"
        >
          <svg class="sidebar-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <path d="M3 4H15M3 9H15M3 14H9" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span class="sidebar-label">{{ t('nav.orders') }}</span>
        </router-link>

        <router-link
          to="/spending"
          class="sidebar-link"
          title="Finance"
          @click="isSidebarOpen = false"
        >
          <svg class="sidebar-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <path d="M9 2V4M9 14V16M4.93 4.93L6.34 6.34M11.66 11.66L13.07 13.07M2 9H4M14 9H16M4.93 13.07L6.34 11.66M11.66 6.34L13.07 4.93" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            <circle cx="9" cy="9" r="3" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span class="sidebar-label">{{ t('nav.finance') }}</span>
        </router-link>

        <router-link
          to="/demand"
          class="sidebar-link"
          title="Demand"
          @click="isSidebarOpen = false"
        >
          <svg class="sidebar-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <path d="M2 13L6 8L9 11L12 6L16 4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M13 4H16V7" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <span class="sidebar-label">{{ t('nav.demandForecast') }}</span>
        </router-link>

        <router-link
          to="/reports"
          class="sidebar-link"
          title="Reports"
          @click="isSidebarOpen = false"
        >
          <svg class="sidebar-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <path d="M4 14V10M7 14V7M10 14V9M13 14V5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            <path d="M2 16H16" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span class="sidebar-label">Reports</span>
        </router-link>
      </nav>

      <div class="sidebar-footer">
        <LanguageSwitcher />
        <ProfileMenu
          @show-profile-details="showProfileDetails = true"
          @show-tasks="showTasks = true"
        />
      </div>
    </aside>

    <!-- Main content area -->
    <div class="main-content">
      <!-- Sticky top bar -->
      <div class="topbar">
        <button
          class="hamburger-btn"
          @click="isSidebarOpen = !isSidebarOpen"
          aria-label="Toggle navigation"
        >
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M3 5H17M3 10H17M3 15H17" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
        </button>
      </div>

      <!-- FilterBar is sticky at top: var(--topbar-height) inside the scroll context -->
      <FilterBar />

      <div class="page-content">
        <router-view />
      </div>
    </div>

    <ProfileDetailsModal
      :is-open="showProfileDetails"
      @close="showProfileDetails = false"
    />

    <TasksModal
      :is-open="showTasks"
      :tasks="tasks"
      @close="showTasks = false"
      @add-task="addTask"
      @delete-task="deleteTask"
      @toggle-task="toggleTask"
    />
  </div>
</template>

<script>
import { ref, onMounted, computed } from 'vue'
import { api } from './api'
import { useAuth } from './composables/useAuth'
import { useI18n } from './composables/useI18n'
import FilterBar from './components/FilterBar.vue'
import ProfileMenu from './components/ProfileMenu.vue'
import ProfileDetailsModal from './components/ProfileDetailsModal.vue'
import TasksModal from './components/TasksModal.vue'
import LanguageSwitcher from './components/LanguageSwitcher.vue'

export default {
  name: 'App',
  components: {
    FilterBar,
    ProfileMenu,
    ProfileDetailsModal,
    TasksModal,
    LanguageSwitcher
  },
  setup() {
    const { currentUser } = useAuth()
    const { t } = useI18n()
    const showProfileDetails = ref(false)
    const showTasks = ref(false)
    const apiTasks = ref([])
    const isSidebarOpen = ref(false)
    const isSidebarCollapsed = ref(false)

    const toggleSidebarCollapse = () => {
      isSidebarCollapsed.value = !isSidebarCollapsed.value
      localStorage.setItem('sidebar-collapsed', String(isSidebarCollapsed.value))
    }

    // Merge mock tasks from currentUser with API tasks
    const tasks = computed(() => {
      return [...currentUser.value.tasks, ...apiTasks.value]
    })

    const loadTasks = async () => {
      try {
        apiTasks.value = await api.getTasks()
      } catch (err) {
        console.error('Failed to load tasks:', err)
      }
    }

    const addTask = async (taskData) => {
      try {
        const newTask = await api.createTask(taskData)
        // Add new task to the beginning of the array
        apiTasks.value.unshift(newTask)
      } catch (err) {
        console.error('Failed to add task:', err)
      }
    }

    const deleteTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const isMockTask = currentUser.value.tasks.some(t => t.id === taskId)

        if (isMockTask) {
          // Remove from mock tasks
          const index = currentUser.value.tasks.findIndex(t => t.id === taskId)
          if (index !== -1) {
            currentUser.value.tasks.splice(index, 1)
          }
        } else {
          // Remove from API tasks
          await api.deleteTask(taskId)
          apiTasks.value = apiTasks.value.filter(t => t.id !== taskId)
        }
      } catch (err) {
        console.error('Failed to delete task:', err)
      }
    }

    const toggleTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const mockTask = currentUser.value.tasks.find(t => t.id === taskId)

        if (mockTask) {
          // Toggle mock task status
          mockTask.status = mockTask.status === 'pending' ? 'completed' : 'pending'
        } else {
          // Toggle API task
          const updatedTask = await api.toggleTask(taskId)
          const index = apiTasks.value.findIndex(t => t.id === taskId)
          if (index !== -1) {
            apiTasks.value[index] = updatedTask
          }
        }
      } catch (err) {
        console.error('Failed to toggle task:', err)
      }
    }

    onMounted(() => {
      loadTasks()
      // Restore collapsed state from localStorage, defaulting to collapsed on narrow screens
      const stored = localStorage.getItem('sidebar-collapsed')
      if (stored !== null) {
        isSidebarCollapsed.value = stored === 'true'
      } else {
        isSidebarCollapsed.value = window.innerWidth < 1024
      }
    })

    return {
      t,
      showProfileDetails,
      showTasks,
      tasks,
      addTask,
      deleteTask,
      toggleTask,
      isSidebarOpen,
      isSidebarCollapsed,
      toggleSidebarCollapse
    }
  }
}
</script>

<style>
/* ===== RESET ===== */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: #f8fafc;
  color: #1e293b;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* ===== DESIGN TOKENS ===== */
:root {
  --sidebar-width: 240px;
  --topbar-height: 56px;
  --sidebar-bg: #0f172a;
  --sidebar-text: #94a3b8;
  --sidebar-text-active: #ffffff;
  --sidebar-active-bg: #1e293b;
  --sidebar-active-border: #2563eb;
  --sidebar-hover-bg: #1e293b;
}

/* ===== APP SHELL ===== */
.app {
  display: flex;
  min-height: 100vh;
}

/* ===== SIDEBAR ===== */
.sidebar {
  position: fixed;
  top: 0;
  left: 0;
  width: var(--sidebar-width);
  height: 100vh;
  background: var(--sidebar-bg);
  display: flex;
  flex-direction: column;
  z-index: 100;
  overflow-y: auto;
  transition: width 0.25s ease;
}

.sidebar-logo {
  padding: 1.25rem 1rem;
  border-bottom: 1px solid #1e293b;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.sidebar-logo-icon {
  flex-shrink: 0;
  display: flex;
  align-items: center;
}

.sidebar-logo-text-block {
  display: flex;
  flex-direction: column;
  min-width: 0;
}

.sidebar-logo-name {
  font-size: 0.9375rem;
  font-weight: 700;
  color: #ffffff;
  letter-spacing: -0.015em;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.sidebar-logo-sub {
  font-size: 0.6875rem;
  color: #64748b;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.sidebar-nav {
  flex: 1;
  padding: 0.75rem 0;
}

.sidebar-link {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.625rem 1rem;
  color: var(--sidebar-text);
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 500;
  border-left: 3px solid transparent;
  transition: background 0.15s, color 0.15s;
}

.sidebar-link:hover {
  background: var(--sidebar-hover-bg);
  color: var(--sidebar-text-active);
}

.sidebar-link.router-link-active {
  background: var(--sidebar-active-bg);
  color: var(--sidebar-text-active);
  border-left-color: var(--sidebar-active-border);
}

.sidebar-icon {
  flex-shrink: 0;
}

.sidebar-footer {
  padding: 0.75rem 1rem;
  border-top: 1px solid #1e293b;
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.sidebar-overlay {
  display: none;
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.4);
  z-index: 99;
}

/* ===== MAIN CONTENT ===== */
.main-content {
  margin-left: var(--sidebar-width);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background: #f8fafc;
  flex: 1;
  transition: margin-left 0.25s ease;
}

/* ===== TOP BAR ===== */
.topbar {
  position: sticky;
  top: 0;
  height: var(--topbar-height);
  background: #ffffff;
  border-bottom: 1px solid #e2e8f0;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  padding: 0 1.5rem;
  gap: 1rem;
  z-index: 90;
  flex-shrink: 0;
}

.hamburger-btn {
  display: none;
  align-items: center;
  justify-content: center;
  width: 36px;
  height: 36px;
  background: none;
  border: 1px solid #e2e8f0;
  border-radius: 6px;
  color: #64748b;
  cursor: pointer;
  margin-right: auto;
  transition: background 0.15s;
}

.hamburger-btn:hover {
  background: #f8fafc;
  color: #0f172a;
}

/* ===== PAGE CONTENT ===== */
.page-content {
  flex: 1;
  padding: 1.5rem;
}

/* ===== RESPONSIVE ===== */
@media (max-width: 767px) {
  :root {
    --sidebar-width: 240px;
  }

  .sidebar {
    transform: translateX(-100%);
    transition: transform 0.25s ease;
  }

  .sidebar.is-open {
    transform: translateX(0);
  }

  .sidebar.is-open ~ .main-content .sidebar-overlay,
  .sidebar-overlay {
    display: block;
  }

  .main-content {
    margin-left: 0;
  }

  .hamburger-btn {
    display: flex;
  }

  .sidebar-label {
    display: block;
  }

  .sidebar-logo-text-block {
    display: flex;
  }

  .sidebar-logo {
    justify-content: flex-start;
  }

  .sidebar-footer {
    align-items: stretch;
  }

  .sidebar-collapse-btn {
    display: none;
  }
}

/* ===== PAGE LAYOUT ===== */
.page-header {
  margin-bottom: 1.5rem;
}

.page-header h2 {
  font-size: 1.875rem;
  font-weight: 700;
  color: #0f172a;
  margin-bottom: 0.375rem;
  letter-spacing: -0.025em;
}

.page-header p {
  color: #64748b;
  font-size: 0.938rem;
}

/* ===== STATS / CARDS ===== */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.25rem;
  margin-bottom: 1.5rem;
}

.stat-card {
  background: white;
  padding: 1.25rem;
  border-radius: 10px;
  border: 1px solid #e2e8f0;
  transition: all 0.2s ease;
}

.stat-card:hover {
  border-color: #cbd5e1;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
}

.stat-label {
  color: #64748b;
  font-size: 0.875rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 0.625rem;
}

.stat-value {
  font-size: 2.25rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
}

.stat-card.warning .stat-value { color: #ea580c; }
.stat-card.success .stat-value { color: #059669; }
.stat-card.danger .stat-value  { color: #dc2626; }
.stat-card.info .stat-value    { color: #2563eb; }

.card {
  background: white;
  border-radius: 10px;
  padding: 1.25rem;
  border: 1px solid #e2e8f0;
  margin-bottom: 1.25rem;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-bottom: 0.875rem;
  border-bottom: 1px solid #e2e8f0;
}

.card-title {
  font-size: 1.125rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
}

/* ===== TABLES ===== */
.table-container {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  background: #f8fafc;
  border-top: 1px solid #e2e8f0;
  border-bottom: 1px solid #e2e8f0;
}

th {
  text-align: left;
  padding: 0.5rem 0.75rem;
  font-weight: 600;
  color: #475569;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

td {
  padding: 0.5rem 0.75rem;
  border-top: 1px solid #f1f5f9;
  color: #334155;
  font-size: 0.875rem;
}

tbody tr {
  transition: background-color 0.15s ease;
}

tbody tr:hover {
  background: #f8fafc;
}

/* ===== BADGES ===== */
.badge {
  display: inline-block;
  padding: 0.313rem 0.75rem;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.025em;
}

.badge.success    { background: #d1fae5; color: #065f46; }
.badge.warning    { background: #fed7aa; color: #92400e; }
.badge.danger     { background: #fecaca; color: #991b1b; }
.badge.info       { background: #dbeafe; color: #1e40af; }
.badge.increasing { background: #d1fae5; color: #065f46; }
.badge.decreasing { background: #fecaca; color: #991b1b; }
.badge.stable     { background: #e0e7ff; color: #3730a3; }
.badge.high       { background: #fecaca; color: #991b1b; }
.badge.medium     { background: #fed7aa; color: #92400e; }
.badge.low        { background: #dbeafe; color: #1e40af; }

/* ===== UTILITIES ===== */
.loading {
  text-align: center;
  padding: 3rem;
  color: #64748b;
  font-size: 0.938rem;
}

.error {
  background: #fef2f2;
  border: 1px solid #fecaca;
  color: #991b1b;
  padding: 1rem;
  border-radius: 8px;
  margin: 1rem 0;
  font-size: 0.938rem;
}

/* ===== SIDEBAR FOOTER COMPONENT OVERRIDES ===== */

/* LanguageSwitcher dark sidebar styling */
.sidebar-footer .language-button {
  background: transparent;
  border-color: #334155;
  color: #94a3b8;
  width: 100%;
  justify-content: flex-start;
}

.sidebar-footer .language-button:hover {
  background: #1e293b;
  border-color: #475569;
  color: #ffffff;
}

.sidebar-footer .globe-icon {
  color: #64748b;
}

/* ProfileMenu dark sidebar styling */
.sidebar-footer .profile-button {
  background: transparent;
  border-color: #334155;
  width: 100%;
  justify-content: flex-start;
}

.sidebar-footer .profile-button:hover {
  background: #1e293b;
  border-color: #475569;
}

.sidebar-footer .profile-name {
  color: #94a3b8;
}

.sidebar-footer .profile-button:hover .profile-name {
  color: #ffffff;
}

.sidebar-footer .chevron {
  color: #475569;
}

/* Dropdowns pop upward in sidebar footer */
.sidebar-footer .profile-menu .dropdown-menu {
  bottom: calc(100% + 0.5rem);
  top: auto;
  right: auto;
  left: 0;
}

.sidebar-footer .language-switcher .dropdown-menu {
  bottom: calc(100% + 0.5rem);
  top: auto;
  right: auto;
  left: 0;
}

/* ===== COLLAPSIBLE SIDEBAR ===== */
.sidebar.is-collapsed {
  width: 64px;
}

.sidebar.is-collapsed .sidebar-label,
.sidebar.is-collapsed .sidebar-logo-text-block {
  display: none;
}

.sidebar.is-collapsed .sidebar-logo {
  justify-content: center;
}

.sidebar.is-collapsed .sidebar-link {
  justify-content: center;
  padding: 0.75rem;
}

.sidebar.is-collapsed .sidebar-footer {
  align-items: center;
  padding: 0.75rem 0.5rem;
}

.sidebar.is-collapsed .sidebar-collapse-btn {
  margin-left: 0;
}

.app.sidebar-collapsed .main-content {
  margin-left: 64px;
}

.sidebar-collapse-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 24px;
  height: 24px;
  margin-left: auto;
  background: none;
  border: none;
  color: var(--sidebar-text);
  cursor: pointer;
  border-radius: 4px;
  flex-shrink: 0;
  transition: color 0.15s, background 0.15s;
}

.sidebar-collapse-btn:hover {
  background: var(--sidebar-hover-bg);
  color: var(--sidebar-text-active);
}

.sidebar-collapse-btn svg {
  transition: transform 0.25s ease;
}

.sidebar.is-collapsed .sidebar-collapse-btn svg {
  transform: rotate(180deg);
}

.sidebar.is-collapsed .sidebar-footer .language-button,
.sidebar.is-collapsed .sidebar-footer .profile-button {
  justify-content: center;
  width: auto;
  padding: 0.5rem;
}

.sidebar.is-collapsed .sidebar-footer .language-label,
.sidebar.is-collapsed .sidebar-footer .profile-name,
.sidebar.is-collapsed .sidebar-footer .chevron {
  display: none;
}
</style>
