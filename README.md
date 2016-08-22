# cchannel
Golang-style channels for C++

### Origins

This was used internally by some [Censys](https://censys.io)-related code. This
is a dump of the code with some minor changes so that it's usable outside of
Censys.

### Building

Easiest is to just copy both source files in the build system for your project.
If this is popular, at some point, I might add a Makefile to this repository
for testing purposes, but for now I'm just supplying the code as is.

### Usage

Dummy example asynchronously processes a vector, and writes the output to a
file.

```cpp
Channel<std::string> to_write;
std::fstream output_file("output.txt");

// Write output to a file as it comes through the queue
std::thread output_thread([&]() {
    for (auto it = to_write.range(); it.valid(); ++it) {
        if (it->size() > 0) {
            output_file << *it;
        }
    }
    output_file.flush();
});

// Read input and send through queue
std::vector<Object> input_data = get_input();
for (const auto& obj : input_data) {
    // Do some work
    std::string = do_something(obj);

    // Sending releases ownership
    to_write.send(std::move(s));
}

output_thread.join();
```

### Support

I'll take pull requests, but I'm not going to provide much other support.

