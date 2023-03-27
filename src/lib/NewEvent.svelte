<script>
  import { NewEventStore } from "./EventStore.js";
  import { DateInput, localeFromDateFnsLocale } from "date-picker-svelte";
  import { enIN } from "date-fns/locale";
  import MultiSelect from "svelte-multiselect";
  import MenuItemSlot from "./MenuItemSlot.svelte";
  import { MenuItems } from "./EventStore";
  let locale = localeFromDateFnsLocale(enIN);
  let handleSubmit = (e) => {
    //e.preventDefaults();
    console.log($NewEventStore);
  };
</script>

<h1>Event Creation Form</h1>
<form class="content">
  <table>
    <tr>
      <td><label for="name" />Name:</td><td><input id="name" type="text" bind:value={$NewEventStore.name} /> </td>
    </tr>
    <tr>
      <td><label for="place" />Place:</td><td><input id="place" type="text" bind:value={$NewEventStore.place} /></td>
    </tr>
    <!-- svelte-ignore a11y-label-has-associated-control -->
    <tr>
      <td><label for="date" />Date:</td>
      <td>
        <DateInput closeOnSelection {locale} format="dd-MM-yyyy HH:mm" placeholder="Select Date 🗓️" bind:value={$NewEventStore.date} />
      </td>
    </tr>
    <tr>
      <td>
        <label for="MenuItems"> Menu Items: </label>
      </td>
      <td>
        <MultiSelect
          id="MenuItems"
          options={MenuItems}
          placeholder="Add menu items..."
          allowUserOptions={false}
          selected={[]}
          bind:value={$NewEventStore.menuItems}
          on:add={(event) => {
            // if you want to persist new user entered custom options to a database, perform
            // a fetch request with type POST here using the event.detail.option payload
            console.log(event.detail.option);
          }}
          required
        >
          <MenuItemSlot let:idx {idx} let:option {option} slot="selected" />
          <MenuItemSlot let:idx {idx} let:option {option} slot="option" />
        </MultiSelect>
      </td>
    </tr>
    <tr>
      <td><label for="host" />Host:</td><td><input id="host" type="text" bind:value={$NewEventStore.host} /> </td>
    </tr>
    <tr>
      <td><label for="phone" />Phone Number:</td><td><input id="phone" type="text" bind:value={$NewEventStore.phone} /> </td>
    </tr>
    <tr>
      <td><label for="count" />Count:</td><td><input id="count" type="text" bind:value={$NewEventStore.count} /> </td>
    </tr>
    <tr>
      <td><label for="totalExpense" />Total Expense:</td><td><input id="totalExpense" type="text" bind:value={$NewEventStore.totalExpense} /> </td>
    </tr>
    <tr>
      <td><label for="receivedAmount" />Received Amount:</td><td><input id="receivedAmount" type="text" bind:value={$NewEventStore.receivedAmount} /> </td>
    </tr>
  </table>
  <button type="submit" on:click|preventDefault={handleSubmit}>Submit</button>
</form>
<p>
  {JSON.stringify($NewEventStore)}
</p>

<style>
  .content {
    display: flex;
    flex-direction: column;
    align-items: start;
  }
  :global(body) {
    padding: calc(5pt + 4vw);
    width: 100%;
    box-sizing: border-box;
    --sms-selected-bg: rgba(40, 0, 80, 0.1);
  }
  label {
    margin: 2em 0 1ex;
  }
</style>
