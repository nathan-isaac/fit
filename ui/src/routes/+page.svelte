<script lang="ts">
import { Repo } from "@automerge/automerge-repo";
import { format, formatISO } from "date-fns";

type Entry = {
	id: string;
	date: Date;
	pounds: number;
};

let entries = $state<Entry[]>([]);

const repo = new Repo({});

const doc = repo.create<{
	entries: Entry[];
}>({
	entries: [],
});

doc.addListener("change", (doc) => {
	console.log("changed", doc);
	entries = doc.doc.entries;
});

function addEntry() {
	const newEntry: Entry = {
		id: Math.random().toString(),
		date: new Date(),
		pounds: 140,
	};

	doc.change((doc) => {
		doc.entries.push(newEntry);
	});
}
</script>

<div class="mx-auto max-w-lg px-4 sm:px-6 lg:px-8 py-14">
    <div>
        <div class="flex items-center justify-between">
            <div class="min-w-0 flex-1">
                <h2 class="text-2xl/7 font-bold text-gray-900 sm:truncate sm:text-3xl sm:tracking-tight">Entries</h2>
            </div>
            <div class="flex shrink-0">
                <button type="button" class="inline-flex items-center rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-xs hover:bg-indigo-500 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
                        onclick={addEntry}
                >New Entry</button>
            </div>
        </div>
    </div>

    <ul role="list" class="divide-y divide-gray-200 mt-4">
        {#each entries as entry}
            <li class="py-4 flex justify-between items-center">
                <div class="text-gray-700">
                    <time datetime={formatISO(entry.date)}>{format(entry.date, "MMM dd, yyyy")}</time>
                </div>
                <div class="flex items-baseline gap-x-1">
                    <span class="text-xl font-semibold tracking-tight">{entry.pounds}</span>
                    <span class="text-sm text-gray-600">lb</span>
                </div>
            </li>
        {/each}
    </ul>
</div>
