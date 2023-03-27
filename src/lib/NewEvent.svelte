<script>
  import { Label, Input, Button } from "flowbite-svelte";
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

<div class=" flex flex-col items-center justify-center gap-9 font-mono ">
  <h1 class="my-4 text-2xl hover:font-bold">Satsang Event Creation Form</h1>
  <div class="grid grid-cols-10 w-full">
    <div class="" />
    <div class="col-span-8">
      <form>
        <div class="grid gap-6 mb-6 md:grid-cols-2">
          <div>
            <div><Label class="block mb-2" for="name" />Name:</div>
            <div><Input class="flex-1" id="name" type="text" bind:value={$NewEventStore.name} /></div>
          </div>

          <div>
            <div><Label class="block mb-2" for="date" />Date:</div>
            <div>
              <DateInput closeOnSelection {locale} format="dd-MM-yyyy" placeholder="Select Date 🗓️" bind:value={$NewEventStore.date} />
            </div>
          </div>
        </div>
        <div class="mb-6">
          <div><Label class="block mb-2" for="place" />Place:</div>
          <div><Input id="place" type="text" bind:value={$NewEventStore.place} /></div>
        </div>
        <!-- svelte-ignore a11y-label-has-associated-control -->

        <div class="mb-6 ">
          <div>
            <Label for="MenuItems" />Menu Items:
          </div>
          <div class="">
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
          </div>
        </div>
        <div class="mb-6">
          <div><Label class="block mb-2" for="host" />Host:</div>
          <div><Input id="host" type="text" bind:value={$NewEventStore.host} /></div>
        </div>
        <div class="grid gap-6 mb-6 md:grid-cols-2">
          <div>
            <div><Label class="block mb-2" for="phone" />Phone Number:</div>
            <div><Input id="phone" type="text" bind:value={$NewEventStore.phone} /></div>
          </div>
          <div>
            <div><Label class="block mb-2" for="count" />Count:</div>
            <div><Input id="count" type="text" bind:value={$NewEventStore.count} /></div>
          </div>
          <div>
            <div><Label class="block mb-2" for="totalExpense" />Total Expense:</div>
            <div><Input id="totalExpense" type="text" bind:value={$NewEventStore.totalExpense} /></div>
          </div>
          <div>
            <div><Label class="block mb-2" for="receivedAmount" />Received Amount:</div>
            <div><Input id="receivedAmount" type="text" bind:value={$NewEventStore.receivedAmount} /></div>
          </div>
        </div>

        <Button gradient color="purpleToBlue" on:click={handleSubmit}>Submit</Button>
      </form>
    </div>
  </div>
  <p>
    {JSON.stringify($NewEventStore)}
  </p>
</div>

<!-- <style>
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
  Label {
    margin: 2em 0 1ex;
  }
</style> -->
