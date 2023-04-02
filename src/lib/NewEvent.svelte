<script>
  import { Label, Input, Button, NumberInput } from "flowbite-svelte";
  import { NewEventStore } from "./EventStore.js";
  import { DateInput, localeFromDateFnsLocale } from "date-picker-svelte";
  import { enIN } from "date-fns/locale";
  import MultiSelect from "svelte-multiselect";
  import MenuItemSlot from "./MenuItemSlot.svelte";
  import { MenuItems } from "./EventStore";
  import * as yup from "yup";

  let locale = localeFromDateFnsLocale(enIN);
  let errors = {};
  const schema = yup.object().shape({
    name: yup.string().required("Name is required."),
    date: yup.date().required("Date is required."),
    menuItems: yup.array().min(1, "Menu items are required."),
    count: yup.number().positive("Count must be positive number."),
    totalExpense: yup.number().positive("Total expense must be positive number."),
    receivedAmount: yup.number().positive("Received amount must be positive number."),
  });

  let handleSubmit = async (e) => {
    e.preventDefault();
    try {
      console.log("inside try");

      await schema.validate($NewEventStore, { abortEarly: false });
      console.log("after validate call");

      errors = {};
    } catch (err) {
      errors = err.inner.reduce((acc, err) => {
        console.log(err);
        return { ...acc, [err.path]: err.message };
      }, {});
    }
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
            {#if errors.name}
              <span class="text-sm text-red-600">{errors.name}</span>
            {/if}
          </div>

          <div>
            <div><Label class="block mb-2" for="date" />Date:</div>
            <div>
              <DateInput closeOnSelection {locale} format="dd-MM-yyyy" placeholder="Select Date 🗓️" bind:value={$NewEventStore.date} />
              {#if errors.date}
                <span class="text-sm text-red-600">{errors.date ? "Date is required." : ""}</span>
              {/if}
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
              required={true}
            >
              <MenuItemSlot let:idx {idx} let:option {option} slot="selected" />
              <MenuItemSlot let:idx {idx} let:option {option} slot="option" />
            </MultiSelect>
            {#if errors.menuItems}
              <span class="text-sm text-red-600">{errors.menuItems}</span>
            {/if}
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
            <div><NumberInput id="count" type="text" bind:value={$NewEventStore.count} /></div>
            {#if errors.count}
              <span class="text-sm text-red-600">{errors.count}</span>
            {/if}
          </div>
          <div>
            <div><Label class="block mb-2" for="totalExpense" />Total Expense:</div>
            <div><NumberInput id="totalExpense" type="text" bind:value={$NewEventStore.totalExpense} /></div>
            {#if errors.totalExpense}
              <span class="text-sm text-red-600">{errors.totalExpense}</span>
            {/if}
          </div>
          <div>
            <div><Label class="block mb-2" for="receivedAmount" />Received Amount:</div>
            <div><NumberInput id="receivedAmount" type="text" bind:value={$NewEventStore.receivedAmount} /></div>
            {#if errors.receivedAmount}
              <span class="text-sm text-red-600">{errors.receivedAmount}</span>
            {/if}
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
