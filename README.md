# photon-pump
A TCP eventstore client in Python 3.6

```python
async def write_an_event():
    async with photonpump.connect() as conn:
        await conn.publish_event('pony_stream', 'pony.jumped', body={
            'name': 'Applejack',
            'height_m': 0.6
        })


async def read_an_event(conn):
    event = await conn.get_event('pony_stream', 1)
    print(event)


async def write_two_events(conn):
    await conn.publish('pony_stream', [
        NewEvent('pony.jumped', body={
            'name': 'Rainbow Colossus',
            'height_m': 0.6
        },
        NewEvent('pony.jumped', body={
            'name': 'Sunshine Carnivore',
            'height_m': 1.12
        })
    ])


async def read_two_events(conn):
    events = await conn.get('pony_stream', max_count=2, from_event=0)
    print events[0]


async def ticker(delay):
    while True:
        yield NewEvent('tick', body{ 'tick': i})
        i += 1
        await asyncio.sleep(delay)


async def write_an_infinite_number_of_events(conn):
    await conn.publish('ticker_stream', ticker(1000))


async def read_an_infinite_number_of_events(conn):
    async for event in conn.stream('ticker_stream'):
        print(event)
```
