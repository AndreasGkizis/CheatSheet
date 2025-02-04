Deleting queues
```bash
count=0

# Get total number of queues (minus the header)
total=$(rabbitmqctl list_queues -q name | tail -n +2 | wc -l)
echo "Found $total queues to delete"

for queue in $(rabbitmqctl list_queues -q name | tail -n +2); do
    rabbitmqctl delete_queue $queue
    ((count++))
    echo "Progress: $count/$total queues deleted"
done

echo "Operation complete. Deleted $count queues."
```
Delete Exchanges
```bash
count=0

# Get total number of exchanges (minus the header and system exchanges)
echo "Listing non-default exchanges..."
total=$(rabbitmqctl list_exchanges -q name | grep -v '^amq\.' | grep -v '^$' | tail -n +3 | wc -l)
echo "Found $total non-default exchanges to delete"

# Delete exchanges, skipping system exchanges (those starting with amq.)
for exchange in $(rabbitmqctl list_exchanges -q name | grep -v '^amq\.' | grep -v '^$' | tail -n +3 | awk '{print $1}'); do
    echo "Attempting to delete exchange: $exchange"
    rabbitmqadmin delete exchange name="$exchange"
    ((count++))
    echo "Progress: $count/$total exchanges deleted"
done

echo "Operation complete. Deleted $count exchanges."
```
