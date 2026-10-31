---
AIGC:
  Label: "1"
  ContentProducer: "001191330110MADHPQQY7L10000"
  ProduceID: "ExportRequest(files=[ExportFile(docType=2, fileType=3)])"
  ReservedCode1: "3187855ddc34ef804f979779c108199d57c3dcfa49d5a33e15e249ee509b1757"
  ContentPropagator: "001191330110MADHPQQY7L10000"
  PropagateID: "ExportRequest(files=[ExportFile(docType=2, fileType=3)])"
  ReservedCode2: ""
---
Complete Design Specification for the AI-Core Programming Language
 
AI-Native Programming Language v0.2
 
(Including detailed explanations of the module system, package management, and the standard library, as well as multi-language interoperability among Rust, Julia, C++, and Python)
 
────────────────────────────────────────────────────────────────────────────
 
Table of Contents
 
────────────────────────────────────────────────────────────────────────────
 
Part One: Language Core
 
【1. Basic Syntax】                     【2. AI-Native Type System】
 
[3. Tensor Operations and Shape Validation] [4. Neural Network Layers (Built-in)] 
【5. Model Definition DSL】                  【6. Automatic Differentiation System (Core Features)】
 
[7. Training Pipeline DSL]                  [8. Multiple Dispatch (referencing Julia)]
 
【9. Hardware Abstraction and Device Management】           【10. Distributed Training】
 
【11. Metaprogramming (refer to Julia)】         【12. Advanced Type System】
 
[13. Memory Management and Ownership] [14. Error Handling]
 
[15. Module System and Visibility] ★ New   [16. Package Management and Build System] ★ New
 
Part Two: Standard Library
 
[17. Overview of the Standard Library] [18. Detailed Explanation of Standard Library Modules] ★New
 
Part Three: Deployment and Toolchain
 
[19. Serialization and Deployment] [20. Toolchain and Command Line] ★ New
 
Part Four: Multilingual Interoperability
 
[21. Interoperability Design Principles and Type Mapping] ★ New (including [21.6–21.10] Memory Interoperability and Memory Safety Switch)
 
【22. Interoperability with Python】            【23. Interoperability with C/C++】★New
 
[24. Interoperability with Rust] ★ New [25. Interoperability with Julia]
 
[26. Interoperability with Other Ecosystems] ★ New
 
Part Five: Custom Code and Extensions
 
[27. Custom Kernels and Inline Assembly] ★ New [28. Custom Operators and Automatic Differentiation Extension] ★ New
 
[29. Custom Compiler Passes and the Backend] ★ New
 
[30. Export AI-Core as a library in other languages] ★ New
 
Part VI: Appendices
 
【31. Compiler Architecture vs. Compilation Models】    【32. Unique Features of AI-Core】
 
【33. Type Inference and Errors】              【34. Performance Optimization Hints】
 
【35. Key Differences from Julia】         【36. Complete Example (Including Interoperability)】
 
[37. Grammar Quick Reference] ★ New addition             [Summary]
 
────────────────────────────────────────────────────────────────────────────
 
Release Notes (Summary of Changes from v0.1 to v0.2)
 
────────────────────────────────────────────────────────────────────────────
 
• Added the [15] module system: mod / use / pub / paths / re-exporting / prelude.
 
• Added a new package management and build system: AI-Core.toml manifest, dependencies (registry/git/path),
 
Multilingual dependency management (Python/C++/Rust/Julia), lockfiles, workspaces, feature flags, and a CLI.
 
• Expanded the [17][18] standard library: extended from 11 modules to a hierarchical namespace, and provided function-level documentation.
 
• Added the [21] Interoperability Overview: ABI, memory ownership, type mapping, and cross-language propagation of errors and exceptions.
 
• Added [21.6–21.10] memory interoperability: raw memory FFI, memory safety modes (off/readonly/sandboxed/full),
 
aicore.ffi.mem library and anti-misoperation mechanisms (capability tokens, boundary checks, double-free/deserialization‑to‑poisoning detection, audit logs).
 
• Rewrite [22] Python Interoperability: @pyimport / py"..." / py{} / Py<T> / DLPack zero-copy /
 
GIL control / callback / packaging as a Python extension.
 
• Added [23] C/C++ interoperability: extern "C", @link, cxx"...", C++ class binding, bindgen,
 
Eigen/OpenCV/CMake integration, custom CUDA operators.
 
• Added [24] Rust interoperability: rust"...", extern crate, #[repr(C)], Cargo integration,
 
Box/Vec ownership is transferred across, and it invokes candle/polars/tokenizers.
 
• Enhanced [25] Julia interoperability: @juliacall / jl"..." / type mapping / embedding libjulia.
 
• Added [26] additional ecosystems: Java (JNI), JS/WASM, Fortran, Go, R, Swift, and ONNX universal interchange.
 
• Added custom code for [27]–[30]: inline assembly, custom operators/plugins, and custom automatic differentiation rules.
 
Custom compiler passes and backends, compiling AI-Core into C ABI, Python, Rust, or Julia libraries.
 
• Added a new [37] grammar quick reference table.
 
════════════════════════════════════════════════════════════════════════════
 
Part One: Language Core
 
════════════════════════════════════════════════════════════════════════════
 
[1. Basic Grammar]
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
x = 10                          Variable assignment (type inference)                 Dynamic typing
 
let x: f32 = 10                 Explicit type declaration                         Type annotation
 
const PI = 3.14159              Compile-time constant                           Type-safe
 
val x = 10                      Immutable binding (similar to Rust's let)            Functional style
 
var x = 10                      Mutable binding                             Mutable variable
 
# Comments                          Single-line comments
 
#= Multi-line comment =#                  Nested multi-line comments
 
;                               Expression separator                         Optional
 
@macro expr                     Macro invocation                               Metaprogramming
 
Supplementary Grammar (v0.2):
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
f"{x} -> {y}"                   String interpolation                           Formatted string
 
r"raw\path"                     Raw string (no escaping)                 Regular expression/path
 
b"bytes"                        byte string                               binary data
 
[x for x in xs if p(x)]         List comprehension                           Data construction
 
(x, y) = (1, 2)                 Destructuring assignment                             Pattern binding
 
x, y = y, x                     Multiple assignment / swap                        Parallel assignment
 
..= 0..=9                       Interval (including the right endpoint)                       Loop/Slicing
 
x |> f |> g                     Pipeline operator                           Chained method calls
 
x?.y                            Safe navigation (null‑coalescing)                 Null-safe
 
_                               Wildcard/Placeholder                          Ignore binding
 
?T                              Optional type shorthand                         Equivalent to Optional{T}
 
if cond { .. } else { .. }      Expression‑style branching (with a return value)              Functional style
 
Block structure description: The AI-Core accepts both curly-brace blocks { } and indented blocks (off-side rule).
 
Consistency must be maintained within the same file; official style guides recommend using curly braces, while interactive REPLs favor indentation.
 
[2. AI-Native Type System]
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
Tensor<T, Shape>                Tensor type (core)                     Tensor<f32, [B, 3, 224, 224]>
 
Tensor<T>                       Shape inference                             Compile-time deduction
 
Tensor<_, [B, 784]>             Partial inference                             Dynamic batching
 
Scalar<T>                       Scalar type                             Scalar<f64>
 
Vector<T, N>                    Fixed-length vector                         Vector<f32, 10>
 
Matrix<T, M, N>                 Fixed-size matrix                         Matrix<f64, 3, 3>
 
Graph<T>                        Computational graph type                           Automatic differentiation graph
 
Device                          Device type                             Device::CUDA(0)
 
Basic scalar types:
 
i8 / i16 / i32 / i64 / i128 / u8 / u16 / u32 / u64 / u128 / isize / usize
 
f16 / bf16 / f32 / f64 / f8e4m3 / f8e5m2
 
bool / char / str / bytes / unit(())
 
complex32 / complex64
 
Shape and Symbol:
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
[N, C, H, W]                    Static shape                             Compile-time constant dimensions
 
[B, _, ...]                     Symbolic dimension (bound at runtime)               The symbol B is determined by the input
 
[*shape]                        Variable length/arbitrary dimensions                       General convolution signature
 
[N, 3, ...]                     Fixed prefix + variable suffix                    Multi-resolution
 
~N                              Constraint dimension (≥1 equality predicate)                 ~N: N > 0
 
Type aliases and opaque types:
 
type Batch = Tensor<f32, [B, 3, 224, 224]>
 
opaque type Handle = usize        # Opaque handle (commonly used in FFI)
 
newtype UserId(i64)               # New type (zero-cost abstraction)
 
[3. Tensor Operations and Shape Validation] 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
x @ y                           Matrix multiplication (using the @ operator)                  A @ B
 
x * y                           Element-wise multiplication (broadcasting)                   Automatic broadcasting
 
x + y                           Element-wise addition                           Shape check
 
x[i, j]                         Indexing access                             Multi-dimensional indexing
 
x[i:j, k]                       Slicing                                 View—no copy
 
x.reshape([B, -1])              Reshapes the array                             -1 automatically computes the size
 
x.transpose(0, 1)               Dimension permutation                             Tensor transposition
 
x.permute([2, 0, 1])            Rearrange arbitrary dimensions                         General transpose
 
x.broadcast_to([B, C, H, W])    Explicit broadcasting                             Expanding dimensions
 
x.sum(axis=1)                   Reduction operation                             Sum along the axis
 
x.mean(), x.max(), x.min()      Statistical operations                             Aggregate functions
 
x.concat(y, axis=0)             Concatenation                                 Dimensional concatenation
 
x.stack([y, z], axis=0)         Stack                                 New dimension
 
Extended Tensor Operator (v0.2):
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
einsum("bij,bjk->bik", x, y)    Einstein summation                         General tensor contraction
 
x.gather(dim, index)            Collects along the specified dimension                             embedding/index
 
x.scatter(dim, index, src)      Scatter along the dimension                             Backpropagation
 
x.where(cond, y)                Conditional selection                             Mask filling
 
x.clamp(min, max)               Numerical clipping                             Gradient clipping
 
x.pad([(0,1),(0,1)])            Padding                                 Same convolution
 
x.roll(shift, axis)             Circular shift                             Position alignment
 
x.einsum / x.outer / x.inner    Inner/outer product                              Broadcasting product
 
x.sort(axis) / x.topk(k)        Sorting/Top-k retrieval                           Retrieval
 
x.norm(p) / x.cosine_sim(y)     Norm/Similarity                          Metric Learning
 
x.cast(f16)                     Type conversion                            Mixed precision
 
x.contiguous()                  Memory contiguization                           Performance
 
x.detach()                      Detach from the gradient                            Freeze
 
x.item()                        Retrieves the scalar value                             Assertion/Log
 
x.to(Device::CUDA(0))           Device transfer                             Hardware scheduling
 
Shape-checking rules (enforced at compile time):
 
• Matrix multiplication: The last dimension of the left tensor must match the second-to-last dimension of the right tensor.
 
• Element-wise: Dimensions can be broadcast (aligned from right to left, with 1s being expanded).
 
• Concatenation: All dimensions except the concatenation axis must be equal.
 
• Specification: the axis must fall within the valid range.
 
[4. Neural Network Layer (Built-in Language)]
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
conv2d(x, out, k=3, s=1, p=1)   2D convolution                              Built-in function
 
linear(x, out_features)         Fully connected layer                             Matrix multiplication + bias
 
batch_norm(x)                   Batch normalization                             Automatically switches between training and inference
 
layer_norm(x)                   Layer Normalization                             Sequence Models
 
relu(x), gelu(x), silu(x)       Activation functions                             Element-wise
 
dropout(x, p=0.5)               Random dropout                             Enabled during training
 
max_pool2d(x, k=2, s=2)         Max pooling                             Downsampling
 
adaptive_avg_pool2d(x, [1, 1])  Adaptive pooling                           Global average
 
attention(Q, K, V)              Self-attention                             Standard attention
 
multihead_attention(Q, K, V, h) Multi-head Attention                           Transformer
 
embedding(idx, vocab, dim)      Word embedding                               Lookup operation
 
positional_encoding(seq, dim)   Positional encoding                             Sine/learnable
 
Extension Layer List (v0.2, see [18]):
 
conv1d / conv3d / conv_transpose2d / depthwise_conv2d
 
group_norm / rms_norm / instance_norm / weight_norm
 
lstm / gru / rnn / bidirectional
 
transformer_encoder_layer / transformer_decoder_layer / cross_attention
 
rope / alibi / flash_attention
 
avg_pool2d / max_pool1d / adaptive_max_pool2d / unfold / fold
 
upsample / pixel_shuffle / pixel_unshuffle
 
bilinear / lora_linear / moe_layer / mixture_of_experts
 
[5. Model Definition DSL]
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
ResNet50 model {                Model definition block                           Declarative architecture
 
layer1: Conv2d(3, 64)       Layer declaration                               Automatic naming
 
fn forward(x) -> Tensor     Definition of the forward pass                         Type annotation
 
x = self.layer1(x)      Layer invocation                               self reference
 
return x
 
}
 
}
 
@model ResNet50                 Macro shorthand                              Equivalent to the above
 
struct Linear {                 Definition of the underlying structure                          Differentiable structure
 
weight: Tensor<f32, [I, O]>
 
bias: Tensor<f32, [O]>
 
fn __call__(x)              Callable object                           Operator overloading
 
return x @ self.weight + self.bias
 
}
 
}
 
Extending Model Capabilities (v0.2):
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
#[derive(Module, Serialize)]    Derive macro                               Automatically implements the trait
 
impl Module for MyBlock         Explicitly implement the module interface                      Custom behavior
 
self.register_buffer(name, t)   Registers a buffer (non-parameter)                  Statistics/Constants
 
self.register_parameter(n, p)   Registers a trainable parameter                        Manual management
 
self.parameters()               Iterate over parameters                             Optimizer/Initialization
 
model.train() / model.eval()    Training/Inference mode                         Affects BN/Dropout
 
model.freeze("layer1")          Freeze the sub-module            Fine-tune
 
model.apply(fn)                 Recursively applies the function                          Custom initialization
 
@tie_weights                    Weight sharing                              embedding+head
 
model.summary()                 Model summary                              Parameter count statistics
 
@compile mode=reduce-overhead   Graph compilation annotation                            Acceleration
 
@dynamic_shape                  Allows dynamic shapes                          Variable-length sequences
 
model.forward_ref(x)            Static graph forward (no Python overhead)          Deployment
 
[6. Automatic Differentiation System (Core Features)]
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
grad(f, x)                      Gradient computation                             Automatic differentiation
 
grad(f, x, y)                   Multivariate gradient                           Higher-order derivatives
 
@grad fn loss(x, y)             Custom gradient                           Overrides the default
 
backward(loss)                  Backpropagation                             Compute graph traversal
 
@no_grad { ... }                Disable gradient context                       Inference optimization
 
@checkpoint fn f(x)             Gradient checkpointing                           Trading memory for computation
 
jacobian(f, x)                  Jacobian matrix                         Vector-valued function 
hessian(f, x)                   Hessian matrix                          Second-order optimization
 
vjp(f, x, v)                    Vector-Jacobian product                      Efficient computation
 
jvp(f, x, v)                    Jacobian–vector product                      Forward mode
 
Extended Differentiation Capability (v0.2):
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
grad(f, x, order=2)             Higher-order derivative                               Second-order optimization
 
grad(f, x, create_graph=true)   Differentiable gradient                             Meta-learning/MAML
 
@autodiff mode=forward          Forward mode                             Small output, large input
 
@autodiff mode=reverse          Reverse mode (default)                      Large output, small input
 
@autodiff mode=mixed            Mixed mode                             Segmented acceleration
 
stop_gradient(x)                Truncate gradients                             Target network
 
@custom_vjp fn f(x)             Custom VJP                           Hand-written backward pass
 
@custom_jvp fn f(x)             Custom JVP                           Hand-written forward pass
 
detach(x) / requires_grad(x)    Gradient control                             Freeze/unfreeze
 
grad_and_value(f, x)            Returns both the gradient and the value                      Monitoring
 
checkpoint_sequential(layers)   Sequential checkpointing                           Ultra-deep networks
 
@nondiff fn argmax(x)           Marks a non-differentiable operator                        Discrete operation
 
[7. Training Workflow DSL]
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
train_config MyConfig {         Training configuration declaration                          Hyperparameter management
 
optimizer: Adam(lr=1e-3)
 
scheduler: CosineAnnealing(T_max=100)
 
loss: CrossEntropyLoss()
 
epochs: 100
 
batch_size: 256
 
device: auto                  # Automatically select GPU/TPU
 
}
 
fn train(model, data, config)   Training function                             Standard training
 
for epoch in 0..config.epochs {
 
for (x, y) in data {
 
@with_grad {          # gradient context 
logits = model(x)
 
loss = config.loss(logits, y)
 
}
 
grad = backward(loss) # Backpropagation
 
config.optimizer.step(grad)
 
config.optimizer.zero_grad()
 
}
 
}
 
Extended Training Capability (v0.2):
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
@accumulate steps=4             Gradient accumulation             Large-batch simulation
 
@amp dtype=bf16                 Automatic mixed precision                          Memory-efficient acceleration
 
@clip_grad max_norm=1.0         Gradient clipping                             Stabilizes training
 
@ema decay=0.999                Exponential moving average                          Weighted smoothing
 
@early_stop patience=5          Early stopping                     Prevents overfitting
 
@resume "ckpt.aic"              Resumable training                             Fault tolerance
 
@wandb / @tensorboard           Experiment logging                             Visualization
 
@lr_find                        Learning Rate Scanning                           Hyperparameter Tuning
 
@profiler { ... }               Performance Profiling                             Bottleneck Identification
 
trainer.fit(model, data, cfg)   Advanced Trainer                         Ready to use out of the box
 
@distributed strategy=...       Distributed (see [10])                    Multi-GPU
 
[8. Multiple Dispatch (referencing Julia)]
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
fn matmul(x: Tensor, y: Tensor) General-purpose method                             Abstract implementation
 
fn matmul(x: Tensor<f32>,        Specialized method (f32)                      Performance optimization
 
y: Tensor<f32>)
 
fn matmul(x: Tensor<f16>,        Specialized method (f16)                      Mixed precision
 
y: Tensor<f16>)
 
-> Tensor<f16>
 
@which matmul(x, y)             View the dispatch method                          Debugging
 
@code_hlo matmul(x, y)          View HLO IR                           XLA intermediate representation
 
@code_mlir matmul(x, y)         View MLIR                             Compiler IR
 
@code_llvm matmul(x, y)         View LLVM IR                          Low-level code
 
@code_ptx matmul(x, y)          View PTX assembly                          CUDA assembly
 
Extended Distribution Capability (v0.2):
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
fn f(x::T) where T<:Number      Parametric method                           Type constraint
 
fn f(x, y) where {T, N}         Multi-parameter constraints                           Combined generic
 
fn f(x::Vector{T}) where T      Container element constraint                         Generic container
 
@dispatch_priority 10           Dispatch priority                           Disambiguation
 
@specialize / @nospecialize     Specialization control                             Compilation size
 
@invoke f(Tuple{Int,Int}, x)    Explicitly invoke a specific method                      Testing/debugging
 
@code_native f(x)               View the machine code                           Extremely optimized
 
@time / @btime f(x)             Timing/Benchmark                            Performance
 
@benchmark f(x)                 Benchmarking statistics            Regression testing
 
[9. Hardware Abstraction and Device Management]
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
device: Device                  Device declaration                             Hardware abstraction
 
fn compute(x: Tensor) -> Tensor Device-agnostic code                          Automatic scheduling
 
.to(Device::CUDA(0))        Explicitly transfer to GPU memory
 
.to(Device::TPU(0))         TPU support                              Cloud training
 
.to(Device::CPU)            CPU fallback                             Debugging/Inference
 
@kernel fn saxpy(a, x, y)       Custom kernel definition                        GPU kernel
 
i = @thread_id()            Thread ID                              CUDA thread
 
y[i] = a * x[i] + y[i]      Element-wise operation                            SIMD
 
@parallel for i in 0..N         Parallel loop                             Automatic parallelism
 
@simd for i in 0..N             SIMD vectorization                           Automatic vectorization
 
@unroll for i in 0..4           Loop unrolling                             Compilation optimization
 
Extended Device Capabilities (v0.2):
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
Device::CUDA(i) / CUDA(0,1)     Multi-GPU device                             Device group
 
Device::ROCM(i)                 AMD GPU                             Heterogeneous
 
Device::METAL / VULKAN          Mobile/Cross-Platform GPU                       On-Device
 
Device::TPU(i)                  TPU                                 Cloud 
Device::NPU(i)                  NPU (Ascend/Cambricon)                     Domestic acceleration
 
Device::MPS                     Apple chip                            macOS
 
device_count()                  Number of devices                             Topology discovery
 
@multi_device [0,1,2,3]         Parallel execution on multiple devices                           Sharding
 
@stream / @event                Streams and Events                             Asynchronous Overlap
 
@cuda_graph                     Capture the compute graph                           Low-latency inference
 
@pin_memory / @prefetch         Memory pinning / prefetching                         Data pipeline
 
@atomic_add(x[i], v)            Atomic operation                             Reduction kernel
 
@shared_mem [256]f32            Shared memory                             Block‑wise matrix multiplication
 
@sync_threads()                 Thread synchronization                             __syncthreads
 
@warp_shuffle(x, lane)          Shuffle within a warp                             Efficient reduction
 
[10. Distributed Training]
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
@distributed strategy=DataParallel Data-parallel annotation Multi-GPU
 
@distributed strategy=ModelParallel # Model parallelism                        Large models
 
@distributed strategy=Pipeline      # Pipeline parallelism                      Multi-stage
 
all_reduce(tensor, op=SUM)      Global reduction                             Gradient synchronization
 
all_gather(tensor)              Global gather                             Batch normalization
 
scatter(tensor, devices)        Data distribution                             Data parallelism
 
gather(tensor, device=0)        Result aggregation                             Inference aggregation
 
Extended Distributed Capabilities (v0.2):
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
@distributed strategy=FSDP      Full-shard data parallelism                        Very large models
 
@distributed strategy=TensorParallel Tensor parallelism                        Column/row slicing
 
@distributed strategy=ExpertParallel MoE Expert Parallelism Sparse Large Model
 
@distributed strategy=ZeRO stage=3   ZeRO optimizer sharding                   Memory optimization
 
reduce_scatter(tensor)          Reduce-scatter                              FSDP
 
broadcast(tensor, src=0)        Broadcasting                                 Initialization
 
all_to_all(tensor)              All-to-all communication               Expert routing
 
barrier()                       Synchronization barrier                             Alignment
 
rank() / world_size()           Process ID / Size                         Conditional logic
 
@shard(dim=0)                   Tensor sharding annotation                           Automatic partitioning
 
@pipeline_stages 4              Number of pipeline stages                           Scheduling
 
@comm_overlap                   Communication-computation overlap                           Hidden latency
 
@checkpoint_reshard             Enable checkpointing                             Saves GPU memory
 
launch(distributed_fn, nproc=8) Launches multiple processes                            Entry point
 
[11. Metaprogramming (referencing Julia)]
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
:(expr)                         Expression reference                           AST node
 
quote ... end                   Long expression reference                         Multi-line
 
$x                              Expression interpolation                           Value embedding
 
:(a + $b)                       Expression with interpolation                        Mixed
 
macro trace(ex)                 Defines a macro                               Compile-time transformation
 
quote
 
println("Entering: " + string(ex))
 
result = $ex
 
println("Result: " + string(result))
 
result
 
end
 
end
 
@trace x + y                    Macro invocation                               Expanded code
 
@macroexpand @trace x + y       View macro expansion                           Debugging
 
@generated fn f(x::T) where T   Generated function                             Type-based generation
 
if T == Float32
 
:(x * 2.0f0)
 
else
 
:(x * 2.0)
 
end
 
end
 
Extended metaprogramming capabilities (v0.2):
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
Expr(:call, :+, 1, 2)           Programmatic construction of the AST                        Code generation
 
eval(expr)                      Runtime evaluation                           Dynamic definition
 
macro_rules! { ... }            Rule macros (pattern matching)                     Syntax extension
 
@derive(Clone, Debug, Eq)       Automatic derivation                             Reduces boilerplate
 
@attr / @attribute              Custom attribute                           Compile-time annotation
 
@include("file.aic")            Text inclusion                             Code reuse
 
@stringify(x)                   Convert to string                             Debug output
 
@concat_idents(a, b)            Identifier concatenation                           Generates a name
 
@compile_time { ... }           Compilation-time execution block                           Constant evaluation/validation
 
@lang("dsl_name")               Embedded DSL entry point                        Syntax extension
 
@doc "..." fn f()               Documentation annotation                             Generate documentation
 
@deprecated("use g")            Deprecation marker                             Migration hint
 
@test / @assert                 Testing and assertions                           Inline testing
 
[12. Advanced Type Systems]
 
trait Differentiable {          Trait definition (similar to a Rust trait)            Interface constraints
 
fn grad(self) -> Self
 
}
 
Implementation of the `Differentiable` trait for `Tensor` – Concrete Type
 
fn grad(self) -> Self { ... }
 
}
 
fn train<T: Differentiable>(    Generic constraint                             Type bound
 
model: T
 
) where T: Module + Serialize
 
abstract type Layer             Abstract type hierarchy                          Type organization
 
struct Conv2d <: Layer          Specific subtype                           Inheritance relationship
 
Union{T1, T2}                   Union type                             Multiple types
 
Optional{T} = Union{T, None}    Optional type                             Null-safe
 
Result<T, E>                    Result type                             Error handling
 
Extended Type Capabilities (v0.2):
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
impl<T: Float> Add for Tensor<T> Generic trait implementation                      Operator overloading
 
trait Module: Serialize + Clone — a supertrait constraint, composition
 
impl Trait for dyn Module       Dynamic dispatch                             Runtime polymorphism
 
dyn Trait / impl Trait          Dynamic/Static Dispatch                         Abstract
 
where T: A + B + 'a             Multiple constraints + lifetime                       Complex bounds
 
associated type Item            Associated type                             Iterator protocol
 
const N: usize                  Associated constant                             Compile-time parameter
 
#[derive(...)]                  Derive                                 Automatically implemented
 
@sealed trait                   Sealed trait                           Prevents external implementations
 
PhantomData<T>                  Phantom type                             Variance/Marker
 
typeclass / instance            Type class (Haskell-style alias)             Mathematical abstraction
 
@auto_trait                     Automatic trait (structural matching)                 Combinatorial interface
 
T: ?Sized                       Relax the Sized constraint                       Slicing/dyn
 
~const                          Constant generic expression                         Compile-time evaluation
 
[13. Memory Management and Ownership]
 
@inbounds x[i]                  Disable bounds checking                         Performance optimization
 
@view x[i:j]                    Creates a view (without copying)                    Memory-efficient
 
@copy x[i:j]                    Explicit copy                             Deep copy
 
@move x                         Ownership transfer (similar to Rust)                Zero-copy
 
@borrow x                       Borrowed reference                             Temporary access
 
pool_allocator(size)            Memory pool allocation                           Reduces fragmentation
 
gradient_checkpointing(fn)      Gradient checkpointing           Memory optimization
 
Expand memory capacity (v0.2):
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
&T / &mut T                     Immutable/mutable borrowing                       Reference
 
'static / 'a                    Lifetime                             Borrow checker
 
Arc<T> / Rc<T>                  Atomic/Regular Reference Counting                     Shared Ownership
 
Box<T>                          Heap allocation                               Recursion/FFI
 
arena { ... }                   Area allocation                             Batch release
 
@noalias / @restrict            No aliasing guarantee                            Compiler optimization
 
@aligned(64)                    Memory alignment                             SIMD/Cache
 
@packed struct                  Compact layout                             FFI-compatible
 
mem_stats()                     Memory statistics                             Tuning
 
@profile_mem { ... }            Memory profiling                             Leak detection
 
out-of-core(x, chunk=1<<20)     Out-of-core / Chunked computation                         For extremely large datasets
 
offload(x, Device::CPU)         Offload to host memory                        Insufficient GPU memory
 
@pin(x)                         Pin memory to the page                     High-speed transfer
 
zero_copy(x)                    Zero-copy view                           Interoperability (see [21])
 
Ownership rules: default move semantics; explicit @borrow for borrowing; @move for transferring; .to() for cross-device conversion. 
A new tensor is created. Borrowing checks are performed at compile time, and ownership at FFI boundaries is managed via handles and a handle table.
 
[14. Error Handling]
 
Result<T, E>                    Result type                             Explicit error
 
Option<T>                       Optional type                             Null safety
 
fn may_fail() -> Result<i32, Error>
 
if condition {
 
Ok(42)
 
} else {
 
Err(Error::NotFound)
 
}
 
}
 
match result {                  Pattern matching                             Exhaustive checking
 
Ok(value) => value,
 
Err(e) => handle(e),
 
}
 
? operator                        Error propagation                             Automatic return of Err
 
let x = may_fail()?             Propagate the error                             Concise syntax
 
panic!("msg")                   Unrecoverable error                         Crashes
 
unreachable!()                  Unreachable code                           Optimization suggestion
 
Extended Error Capability (v0.2):
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
try { ... } catch e { ... }     Exception-style catching                           Script-friendly
 
finally { ... }                 Cleanup block                               Resource release
 
defer { close(f) }              Delayed execution                             Similar to Go's defer
 
error! / warn! / info!          Diagnostic macro                               Log
 
@track_caller                   Records the call site                           Friendly error reporting
 
thiserror / anyhow style         Error enum / context                         Library design
 
Error::from(e) / e.context(..) Error conversion/wrapping Chain
 
assert!(cond, "msg")            Assertion                                 Preconditions
 
debug_assert!                   Debug-only assertion                             Zero overhead in release builds
 
@must_use                       The return value must be used                          To prevent misuse
 
catch_unwind(f)                 Captures a panic (required at the FFI boundary)              Interoperability-safe
 
Cross-language error propagation: Errors at FFI boundaries are uniformly mapped to Result (see [21.5] for details).
 
[15. Module System and Visibility] (Added in v0.2)
 
AI-Core’s module system is used to organize large projects, manage visibility, and support library reuse.
 
15.1 Module Definition
 
mod nn {                         Inline module definition
 
pub struct Linear { ... }    pub indicates visibility to the outside world
 
pub fn relu(x) -> Tensor { ... }
 
fn internal_init() { ... }   Default is private (visible only within the current module)
 
}
 
mod data;                        External module (loaded from src/data.aic or src/data/mod.aic)
 
15.2 Importing and Paths
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
use path::Item                  Import a single item                             use aicore::nn::Linear
 
use path::{A, B}                Import multiple items                             use aicore::nn::{Linear, ReLU}
 
use path::*                     Wildcard import                             use aicore::nn::*
 
use path as alias               Alias import                             use aicore::nn as neural
 
pub use path::Item              Re-export                   pub use aicore::nn::Linear
 
use self::sub::f                Reference within the current module                         self::utils::helper
 
use super::f                    Reference to the parent module                           super::shared
 
use crate::config::Cfg          Package root reference                             crate::config::Cfg
 
use ::aicore::core::Tensor      Absolute path (external crate)                 ::aicore::...
 
15.3 Visibility Modifiers
 
Grammar Point                          Function
 
──────────────────────────────────────────────
 
(Default)                          Private: Visible only to the current module and its submodules
 
pub                             Fully public
 
pub(crate)                      Visible only within the current crate
 
pub(super)                      Visible only in the parent module
 
pub(in crate::nn)               Visible only within the specified path
 
pub(self)                       Equivalent to private
 
15.4 Module File Layout
 
src/
 
├── main.aic            # Executable entry point (fn main)
 
├── lib.aic             # Library entry point (defines the package’s root module) 
├── nn/ 
│   ├── mod.aic         # Submodule nn
 
│   ├── conv.aic 
│   └── attention.aic
 
├── data.aic 
└── utils/
 
└── mod.aic
 
15.5 Pre-import and Agreements
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
prelude                         Commonly used items automatically imported                      Tensor, Device, grad
 
#![no_prelude]                  Disable pre-imports                            Ultimate control
 
#![warn(missing_docs)]          Package-level lint configuration                        Quality
 
#[path = "custom.aic"] mod x    Specifies the module file path                      Non-conventional layout
 
#[cfg(feature = "cuda")]        Conditional compilation                             Feature flag
 
#[cfg(target = "cuda")]         Target condition                             Hardware branch
 
15.6 Namespaces and Packages
 
• A “package” is described by an AI-Core.toml file.
 
• A package can contain multiple “targets”: lib, bin, test, bench, example,
 
python-ext, cdylib (see [16], [30]).
 
• The package name must be globally unique within the registry, and the module name must be unique within its package.
 
• Naming conventions: modules use snake_case, types use CamelCase, and constants use SCREAMING_SNAKE_CASE.
 
[16. Package Management and Build Systems] (Added in v0.2)
 
16.1 Manifest File AI-Core.toml
 
[package]
 
name        = "resnet-toolkit"
 
version     = "0.2.0"
 
edition     = "2026"
 
authors     = ["Jason <jason@example.com>"]
 
license     = "Apache-2.0"
 
description = "ResNet training toolkit in AI-Core"
 
repository = "https://github.com/example/resnet-toolkit"
 
keywords    = ["vision", "resnet", "training"]
 
readme      = "README.md"
 
# ── AI-Core Dependencies ───────────────────────────────────────────── 
[dependencies]
 
aicore-nn      = "0.3"                       # registry semantic version
 
aicore-metrics = { version = "0.1", features = ["cuda"] }
 
local-utils    = { path = "../utils" }        # Local path
 
fast-ops       = { git = "https://github.com/x/fast-ops", tag = "v1.2.0" }
 
private-lib    = { registry = "https://ai-core.internal/registry" }
 
[dev-dependencies]
 
aicore-bench = "0.1"
 
# ── Multilingual Dependencies (Interoperability, see Section 4)─────────────────────────
 
[python.dependencies] 
numpy        = ">=1.24" 
torch        = { version = "2.3", index = "pypi" }
 
transformers = { git = "https://github.com/huggingface/transformers", rev = "main" }
 
local-py     = { path = "./pyutils" }
 
[cxx.dependencies] 
eigen   = { version = "3.4", system = true }               # System already installed
 
opencv  = { version = "4.9", headers = "/usr/include/opencv4",
 
libs = ["opencv_core", "opencv_imgproc"] }
 
myblas  = { path = "./vendor/blas", build = "cmake" }
 
[rust.dependencies] 
ndarray     = { crate = "ndarray",  version = "0.15", features = ["rayon"] }
 
tokenizers  = { crate = "tokenizers", version = "0.19" }
 
polars      = { crate = "polars",   version = "0.40", features = ["lazy"] }
 
[julia.dependencies] 
SpecialFunctions     = "2.4"
 
DifferentialEquations = "7.10"
 
CUDA                 = "5.2" 
# ── Construction and Features ─────────────────────────────────────────────── 
[build]
 
opt-level    = 3
 
target       = ["cuda", "x86-64-v4"]
 
backends     = ["mlir", "llvm", "cuda"]
 
link-time-opt = true
 
[features]
 
default = ["cuda"]
 
cuda    = ["aicore-nn/cuda", "python:torch-cuda"]
 
tpu     = ["aicore-distributed/tpu"]
 
debug-grad = []                # Custom feature
 
[[bin]]
 
name = "train"
 
path = "src/main.aic"
 
[[test]]
 
name = "integration"
 
path = "tests/integration.aic"
 
[[bench]]
 
name = "resnet"
 
path = "benches/resnet.aic"
 
16.2 Command-line tool aicore
 
Command                            Function
 
──────────────────────────────────────────────
 
aicore new <name>               Create a new project (--lib / --bin / --mixed)
 
aicore init                     Initialize a project in the current directory
 
aicore add <pkg>                Add a dependency (use --python/--cxx/--rust/--julia to specify the ecosystem)
 
aicore remove <pkg>             Remove dependency
 
aicore build                    Compile (with --release / --target <backend>)
 
aicore run                      Compile and run
 
aicore test                     Run tests (--doc / --coverage)
 
aicore bench                    Run benchmark
 
aicore fmt                      Format (with --check, only checks)
 
aicore lint                     Static analysis (Clippy style)
 
aicore doc                      Generate documentation (--open)
 
aicore check                    Quick type/shape check (does not generate code)
 
aicore tree                     dependency tree
 
aicore update [pkg]             Update dependencies
 
aicore publish                  Publish to the registry
 
aicore vendor                   Depends on localization (offline build)
 
aicore clean                    Clean up artifacts
 
aicore repl                     Interactive REPL
 
aicore notebook                 Launch a Jupyter-style notebook
 
aicore doctor                   Environmental self-check (CUDA/Python/C++ toolchain)
 
aicore python install           Install Python dependencies (see [python.dependencies])
 
aicore cxx gen-bindings         Generate bindings (bindgen) from C++ header files
 
aicore rust vendor              Synchronize Cargo dependencies
 
aicore julia instantiate        Synchronize Julia dependencies
 
aicore export                   Export the model (ONNX/TensorRT; see [19])
 
16.3 Locking Files and Reproducible Builds
 
• AI-Core.lock records the exact version/commit hash and checksum for each dependency.
 
• Cross-language dependencies are similarly locked: Python (pip hash), C++ (header/library hashing),
 
Rust (Cargo.lock), Julia (Manifest.toml).
 
• aicore build --frozen Forces the use of locked versions, intended for CI/production.
 
16.4 Workspace
 
[workspace] 
members = ["core", "nn", "data", "cli"]
 
resolver = "2"
 
# Shared dependencies and build caching, unified version across packages
 
Version 16.5 and Release
 
• Semantic versioning follows the MAJOR.MINOR.PATCH format; breaking changes increment the MAJOR version.
 
• Features are used for optional functionality and conditional compilation.
 
• yank can revert to a problematic version; the registry supports images and private registries.
 
• Released artifacts may include: source code packages, precompiled libraries, Python wheels, C header files,
 
Rust crate, Julia package (see [30]).
 
════════════════════════════════════════════════════════════════════════════
 
Part Two: Standard Library
 
════════════════════════════════════════════════════════════════════════════
 
[17. Standard Library Overview]
 
The AI-Core standard library employs a hierarchical namespace. The core layer is minimalist and dependency-free, while the domain layer is imported on demand.
 
Hierarchy            Module                             Function
 
──────────────────────────────────────────────
 
Core            aicore.core                      Basic types, Option/Result, iterators, traits
 
aicore.tensor                    Tensor creation/operations/shapes/einsum
 
aicore.autograd                  Automatic differentiation engine
 
aicore.math                      Mathematical functions (transcendental and elementary functions)
 
aicore.stats                     Statistics and Probability Distributions
 
aicore.random                    Random number generation (GPU-accelerated)
 
aicore.linalg                    Linear algebra (beyond BLAS)
 
aicore.parallel                  Parallel/Asynchronous/Thread Pool
 
aicore.time                      Time/Timer/Scheduling
 
aicore.io                        File/Stream/Network IO
 
aicore.serialize                 Serialization (bin/json/msgpack)
 
aicore.log                       Logs
 
aicore.test                      Testing framework
 
Neural Networks        aicore.nn                        Layers, Activation, Initialization, Loss
 
aicore.nn.functional             Stateless functional API
 
aicore.optim                     Optimizer and Scheduler
 
aicore.loss                      Loss function
 
aicore.init                      Parameter initialization
 
aicore.metrics                   Evaluation metrics
 
Data and Domain      aicore.data                      Dataset/DataLoader/Sampling/Transformations
 
aicore.text                     Tokenization/Embedding/Text Tools
 
aicore.vision                    Image processing/enhancement/pre-trained models
 
aicore.audio                     Audio processing/spectrum
 
aicore.rl                        Reinforcement learning environment/algorithm
 
aicore.graph                     GNN/graph data structure
 
System and Deployment      aicore.distributed               Distributed Training (DDP/FSDP/Pipeline)
 
aicore.jit                       JIT compilation (trace/script/compiler cache)
 
aicore.export                    Model export (ONNX/TensorRT/CoreML)
 
aicore.profiler                  Performance/Memory/Communication Profiling
 
aicore.deploy                    Inference service / Batch processing / Quantization
 
Interoperability          aicore.ffi                       Cross-language runtime (see Section 4 for details)
 
aicore.ffi.py                    Python bridge
 
aicore.ffi.cxx                   C/C++ bridge
 
aicore.ffi.rs                    Rust bridge
 
aicore.ffi.jl                    Julia bridge
 
Convention: aicore.core is automatically brought into the prelude; other modules must be explicitly imported using `use`.
 
Example: use aicore::nn::{Linear, ReLU}; use aicore::optim::Adam;
 
[18. Detailed Explanation of Standard Library Modules] (Added in v0.2)
 
18.1 aicore.core
 
Types: Option<T>, Result<T,E>, Vec<T>, HashMap<K,V>, Box<T>, Arc<T>, Range, Slice
 
trait: Clone, Copy, Debug, Display, Eq, Ord, Hash, Iterator, IntoIterator,
 
From, Into, TryFrom, Serialize, Deserialize
 
Functions: assert!/assert_eq!, panic!, todo!, unreachable!, print!/println!,
 
format!, min/max/clamp, abs, len, zip, enumerate, map/filter/fold
 
18.2 aicore.tensor
 
Create: zeros, ones, full, eye, arange, linspace, randn, rand, randint,
 
from_list, from_numpy(see [22]), as_tensor
 
Shape: reshape, view, permute, transpose, squeeze, unsqueeze, expand,
 
broadcast_to, flatten, cat, stack, split, chunk, tile, repeat
 
Operations: matmul, einsum, outer, inner, tensordot, conv* (see nn), where, gather,
 
scatter, index_select, masked_fill, pad, roll, flip, sort, topk, unique
 
Functions: sum, mean, prod, max, min, argmax, argmin, var, std, cumsum, norm
 
Attributes: shape, dtype, device, ndim, numel, stride, is_contiguous, T, grad
 
Conversion: to(device), cast(dtype), contiguous, cpu, cuda, numpy, item, tolist
 
18.3 aicore.nn (Excerpt)
 
Layers: Linear, Conv1d/2d/3d, ConvTranspose2d, BatchNorm1d/2d, LayerNorm,
 
GroupNorm, RMSNorm, InstanceNorm, Dropout, Embedding, LSTM, GRU, RNN,
 
MultiheadAttention, TransformerEncoderLayer, TransformerDecoderLayer,
 
MaxPool2d, AvgPool2d, AdaptiveAvgPool2d, Upsample, PixelShuffle, LoRA
 
Activation: ReLU, LeakyReLU, GELU, SiLU/Swish, Mish, Tanh, Sigmoid, Softmax,
 
LogSoftmax, ELU, SELU, GLU, SwiGLU, GeGLU
 
Initialization: kaiming_uniform, kaiming_normal, xavier_uniform, xavier_normal,
 
orthogonal, trunc_normal, zeros_, ones_
 
Containers: Sequential, ModuleList, ModuleDict, ParameterList
 
Tools: functional (F.relu, F.conv2d, F.linear, F.softmax, F.pad, F.interpolate)
 
18.4 aicore.optim
 
Optimizer: SGD, Adam, AdamW, Adamax, Adagrad, RMSprop, Adadelta, Nadam,
 
LAMB, LARS, Lion, Sophia, RAdam, Shampoo
 
Scheduler: StepLR, MultiStepLR, ExponentialLR, CosineAnnealingLR,
 
CosineAnnealingWarmRestarts, OneCycleLR, ReduceLROnPlateau,
 
LambdaLR, LinearWarmup, PolynomialDecay
 
Tools: clip_grad_norm_, clip_grad_value_, zero_grad, param_groups
 
18.5 aicore.loss
 
mse_loss, l1_loss, smooth_l1_loss, huber_loss, cross_entropy,
 
binary_cross_entropy, nll_loss, kl_div, focal_loss, dice_loss, iou_loss,
 
ctc_loss, contrastive_loss, triplet_margin_loss, cosine_embedding_loss,
 
label_smoothing, soft_dice, tversky_loss
 
18.6 aicore.data
 
Dataset, IterableDataset, DataLoader, Sampler, SequentialSampler,
 
RandomSampler, WeightedRandomSampler, DistributedSampler, BatchSampler,
 
collate_fn, worker_init, pin_memory, prefetch_factor
 
Transforms: Resize, CenterCrop, RandomCrop, RandomFlip, Normalize,
 
ToTensor, Compose, RandAugment, MixUp, CutMix, ColorJitter
 
Tools: train_test_split, k_fold, streaming, shard, webdataset
 
18.7 aicore.text
 
tokenize, detokenize, BPE, WordPiece, SentencePiece, ByteLevelBPE,
 
vocab, pad_sequence, truncate, attention_mask, positional_encoding, rope,
 
sinusoidal, embedding_bag, ngram, tfidf, bleu, rouge, perplexity
 
18.8 aicore.vision
 
read_image, write_image, resize, crop, rotate, flip, normalize,
 
to_grayscale, color_convert, box_iou, nms, roi_align, draw_boxes,
 
pretrained (resnet50, vit_b_16, efficientnet_b0, yolo), feature_extract
 
18.9 aicore.audio
 
load_audio, save_audio, resample, mel_spectrogram, mfcc, stft, istft,
 
spectrogram, augment_audio, vad, speaker_embedding, asr_infer, tts_synth
 
18.10 aicore.rl
 
Env, Gym, EnvWrapper, reset, step, replay_buffer, prioritized_replay,
 
ppo, dqn, sac, a2c, a3c, ddpg, td3, gae, epsilon_greedy, rollout
 
18.11 aicore.distributed
 
init_process_group, destroy_process_group, DDP, FSDP, Pipeline,
 
TensorParallel, ExpertParallel, ZeRO, ProcessGroup, all_reduce, all_gather,
 
reduce_scatter, all_to_all, broadcast, barrier, rank, world_size, launch
 
18.12 aicore.jit
 
trace, script, compile, jit, graph, export_graph, load_graph, cache,
 
fuse, optimize, dynamic, static, recompile, set_compile_config
 
18.13 aicore.export
 
export_onnx, export_torchscript, export_tflite, export_tensorrt,
 
export_coreml, export_openvino, export_stablehlo, export_mlir,
 
export_savedmodel, quantize, prune, fuse_conv_bn, calibrate
 
18.14 aicore.profiler 
profile, record_function, memory_summary, flops, timeline, cuda_events,
 
trace_handler, memory_snapshot, comm_profile, kernel_stats, roofline
 
18.15 aicore.ffi (Interoperability runtime, see Part Four)
 
py_import, py_eval, py_call, to_py, from_py, gil_acquire, gil_release,
 
cxx_include, cxx_compile, cxx_call, rust_crate, rust_call, jl_using,
 
jl_call, load_library, dlopen, dlsym, extern_handle, marshal, unmarshal
 
18.15.1 aicore.ffi.mem (memory interoperability library; see [21.6–21.10])
 
# Safety Abstractions: Buffer<T>, Span<T>, Span<const T>, Span<mut T>, RawPtr<T>,
 
#           MemoryView<T>, SharedBuffer, Handle
 
# Allocation: alloc, alloc_zeroed, alloc_pinned, alloc_device, realloc, free
 
# Views: from_raw_parts, from_cbuffer, from_dlpack, to_dlpack, from_mmap, shm_open
 
# Validation: validate, is_valid, check_device, guard_page, stats, leaks, dump, poison
 
# Token: capability(MemCap::Raw|MemCap::ReadOnly|MemCap::Device)   # Memory switching must be enabled
 
18.16 Other
 
aicore.math: exp, log, sqrt, pow, sin, cos, tan, erf, gamma, lgamma,
 
bessel_j, beta, sigmoid, softplus, logsumexp, erf_inv
 
aicore.stats: normal, uniform, bernoulli, categorical, dirichlet, poisson,
 
beta, gamma, mean, median, quantile, corr, cov, histogram
 
aicore.serialize: save, load, dumps, loads (bin/json/msgpack/safetensors)
 
aicore.log: debug, info, warn, error, logger, log_to_file, log_to_tb
 
aicore.test: test, assert_eq, assert_approx, fixture, mock, parametrize,
 
bench, snapshot, coverage
 
aicore.parallel: spawn, join, channel, mutex, rwlock, atomic, thread_pool,
 
async, await, task, select
 
aicore.time: now, elapsed, sleep, timeout, timer, cron, schedule
 
aicore.io: read, write, open, read_lines, buffered, mmap, http_get,
 
http_post, websocket, s3_get, hdfs_read
 
════════════════════════════════════════════════════════════════════════════
 
Part Three: Deployment and Toolchain
 
════════════════════════════════════════════════════════════════════════════
 
[19. Serialization and Deployment]
 
serialize(model, "model.aic")   Serialize the model                           Save weights/structure
 
deserialize("model.aic")        Deserialization                             Load the model
 
export_onnx(model, "model.onnx") exports the model to ONNX format, enabling cross-framework compatibility. 
export_torchscript(model)       Exports to TorchScript                      PyTorch-compatible
 
export_tflite(model)            Export to TFLite                           For mobile devices
 
export_tensorrt(model)          Export to TensorRT                         NVIDIA inference
 
quantize(model, bits=8)         Quantization                                INT8 inference
 
prune(model, sparsity=0.5)      Pruning                                Sparse model
 
fuse_conv_bn(model)             Operator fusion                             Inference optimization
 
Extended Deployment Capabilities (v0.2):
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
export_stablehlo(model)         Export StableHLO                       Cross-compiler
 
export_openvino(model)          Export to OpenVINO                        Intel inference
 
export_coreml(model, "m.mlpackage") Export to CoreML                       Apple
 
export_gguf(model)              Export to GGUF                            Local LLM
 
save_safetensors(model, path)   Safe Tensors format                          Large model weights
 
quantize(model, scheme="awq")   Weight quantization                             LLM compression
 
quantize(model, scheme="gptq")  Quantization after training                            4-bit
 
distill(student, teacher)       Knowledge Distillation                             Small Model
 
serve(model, port=8080)         Inference service (REST/gRPC)                 Deploy
 
batch_infer(model, stream)      Streaming batch inference                            Low latency
 
@onnx_opset 18                  Specifies the ONNX version                        Compatible
 
onnx_verify(path, inputs)       Export verification                             Numerical alignment
 
deploy_wasm(model, "m.wasm")    Compiles to WASM                           Browser side
 
deploy_mobile(model, "android") Mobile packaging                            Client-side
 
compile_ahead(model, target)    AOT compilation                             No JIT deployment
 
[20. Toolchain and Command Line] (Added in v0.2)
 
Tool                            Function
 
──────────────────────────────────────────────
 
aicore (CLI) Project management, building, testing, and releasing (see [16.2])
 
aicore repl                     Interactive REPL (supports real-time type/shape feedback)
 
aicore notebook                 Jupyter kernel (.ipynb)
 
aicore fmt / aicore lint        Formatting and static analysis
 
aicore doc                      Document generation (HTML/Markdown)
 
aicore test --coverage          Coverage rate
 
aicore bench                    Benchmark and Performance Regression
 
aicore profile <script>         One-click performance profiling (generates a flame graph)
 
aicore debug <script>           DAP debugger
 
aicore lsp                      Language server (VS Code / Neovim / JetBrains)
 
aicore doctor                   Toolchain self-check (CUDA/Python/C++/Rust/Julia)
 
aicore python install           Install Python dependencies
 
aicore cxx gen-bindings         Generate C++ bindings
 
aicore publish                   Publish to the registry
 
Editor support: syntax highlighting, code completion, navigation, hover tooltips for types and shapes, inline error marking, formatting,
 
Notebook integration, shape visualization (tensor dimension diagram).
 
════════════════════════════════════════════════════════════════════════════
 
Part Four: Multilingual Interoperability
 
════════════════════════════════════════════════════════════════════════════
 
[21. Interoperability Design Principles and Type Mapping] (Added in v0.2)
 
21.1 Design Principles
 
1. Zero-copy priority: Tensors/arrays share memory via the DLPack/buffer protocol without copying.
 
2. Explicit Ownership: Cross-boundary ownership transfers must be explicit (@move / handle / Box).
 
Borrowing is checked at runtime; objects on the C/C++/Rust side are held as opaque handles.
 
3. Boundary Type Safety: All cross-language calls undergo type-mapping checks at compile time, and an error is reported if a mapping cannot be performed.
 
4. Failures are reversible: Failing calls return a Result; exceptions/panics are caught and converted into an Err.
 
5. Unified Runtime: aicore.ffi provides a unified handle table, memory pool, GIL/lock management, and an exception bridge.
 
21.2 Comprehensive Type Mapping Table (AI-Core → Target Language)
 
AI-Core          Python        C / C++            Rust              Julia
 
──────────────────────────────────────────────────────────────────────────
 
i32 / i64        int           int32_t/int64_t    i32/i64           Int32/Int64
 
u8..u64          int           uint*_t            u8..u64           UInt*
 
f32 / f64        float         float/double       f32/f64           Float32/Float64
 
f16 / bf16       numpy.float16 __half / bf16      half::f16         Float16
 
bool             bool          bool               bool              Bool
 
char / str       str           char* / std::string &str / String     String
 
bytes            bytes         uint8_t* / vector  &[u8] / Vec<u8>   Vector{UInt8}
 
unit ()          None          void               ()                Nothing
 
Vec<T>           list          std::vector<T>     Vec<T>            Vector{T} 
HashMap<K,V>     dict          std::unordered_map HashMap<K,V>      Dict{K,V}
 
Option<T>        Optional[T]   std::optional<T>   Option<T>         Union{T,Nothing}
 
Result<T,E>      Exception     (errno / throw)    Result<T,E>       Union{T,Exception}
 
Tensor<T,S>      numpy.ndarray (DLPack) / torch.Tensor  (DLPack)  Array{T,N} / CuArray
 
Device           "cuda:0"      cudaStream_t       Device            CuDevice
 
PyObject         object        —                  —                 —
 
21.3 The Memory Ownership Model
 
• Input borrowing: By default, AI-Core does not manage the lifetime; it remains valid for the duration of the call.
 
• Transfer (@move / into_*): Ownership is transferred to the callee, and the handle on the AI-Core side becomes invalid.
 
• Sharing (Arc / Reference Counting): Cross-boundary reference counting, with both sides able to hold references.
 
• Copy (@copy / clone): An explicit deep copy, safe but with overhead.
 
• Handle (Handle<T>): External objects are stored in the FFI handle table as integer handles, on the AI-Core side.
 
It holds only a handle; the handle can be released by `Handle::free()` or `defer`.
 
21.4 Calling Conventions
 
• The C ABI is a common interface across languages (extern "C").
 
• Complex types are represented using a struct pointer plus a length descriptor (the slice structure {ptr, len}).
 
• Tensors are passed using a DLPack capsule (dtype/shape/strides/device/ptr).
 
• Strings are uniformly encoded in UTF-8 and passed as (ptr, len), without relying on a NUL terminator.
 
21.5 Cross-Language Propagation of Errors and Exceptions
 
Boundary                            Mechanism
 
──────────────────────────────────────────────
 
AI-Core ← Python                Python Exception → PyErr → Result::Err
 
AI-Core ← C/C++                 errno / Return Codes / C++ Exceptions (catch) → Err
 
AI-Core ← Rust                  Rust panic（catch_unwind）/ Result → Err
 
AI-Core ← Julia                 Julia Exception → JLError → Result::Err
 
Python/C++/Rust/Julia ← AI-Core AI-Core panic/Err → Target-language exception/error code
 
Rule: FFI boundaries prohibit panics from crossing the ABI (UB); panics must be caught and converted at the boundary.
 
────────────────────────────────────────────────────────────────────────────
 
21.6 Raw Memory Interop ★ Added in v0.2
 
────────────────────────────────────────────────────────────────────────────
 
Usage: Sharing raw memory with C/C++/Rust/Python, zero-copy large buffers, mmap/shared memory,
 
External allocators, GPU video memory, DMA, embedded systems, and environments without a runtime. 
Risk (must be intercepted by the language mechanism):
 
Dangling pointers, buffer overflows, double free, use-after-free, type confusion,
 
Alignment errors, integer overflows in size*count operations, mixing CPU and GPU pointers, casting integers to pointers, writing to read-only memory regions,
 
Race conditions and lifecycle mismatches in inter-process shared memory.
 
Design Principles:
 
• Disabled by default: When the “memory switch” is not explicitly enabled, any raw memory operations result in a compilation error.
 
• Explicit authorization: The raw memory API requires a “Capability Token” to be invoked.
 
• Abstract First: Prefer using safe abstractions such as Buffer and Span, which carry length and access‑control information, over raw pointers.
 
• Auditability: Every raw memory operation can be logged, facilitating traceability and circuit breaking.
 
• Can only be tightened, not relaxed: Environment variables and runtime switches can only tighten permissions; they cannot relax compile-time settings.
 
────────────────────────────────────────────────────────────────────────────
 
21.7 Memory Safety Switch (Memory Switch) ★ v0.2 Added
 
────────────────────────────────────────────────────────────────────────────
 
Switch level (from weak to strong, default: off):
 
Level            Meaning
 
──────────────────────────────────────────────
 
off (default): Disallows all raw pointers; only safe abstractions such as Buffer and Span are permitted.
 
readonly        Allows only read access to external memory; prohibits allocation, writing, and deallocation
 
sandboxed       Allows read and write operations within the registered/controlled area; prohibits access to arbitrary addresses
 
full            Allows arbitrary addressing, allocation, and deallocation (equivalent to unsafe; requires a token and auditing)
 
Switch position and syntax:
 
Switch Position        Syntax                                              Scope
 
──────────────────────────────────────────────────────────────
 
Manifest file        [memory] enabled = true / level = "sandboxed"     Entire package
 
crate attribute      #![memory(level = "full")]                        The entire crate
 
Module/Function Attribute    #[memory(level = "readonly")]                     This module/function
 
Scope block        @memory_gate(level = "sandboxed") { ... }         Lexical scope
 
Runtime coverage      AICORE_MEMORY=allow / deny / level:readonly        Can only be tightened
 
Environment variable AICORE_MEMORY_DENY=1 – Enforces disabling (CI/production) 
AI-Core.toml configuration:
 
[memory]
 
enabled         = false          # Disabled by default; must be explicitly enabled
 
level           = "sandboxed"    # off | readonly | sandboxed | full
 
audit           = true           # Log every raw memory operation
 
allow_raw_addr  = false          # Whether to allow arbitrary addresses (only available at the full level)
 
deny_env_override = true         # Prohibit relaxing permissions via environment variables
 
poison_on_free  = true           # Poison after free, detects use-after-free
 
Capability Token:
 
let cap = mem::capability(MemCap::Raw)?      # If the switch is not enabled → compilation error/returns Err
 
mem::alloc(1024, align = 64, cap)?           # The raw memory API must include a cap
 
MemCap Category: Raw (arbitrary read/write), ReadOnly (read-only), Device(dev) (dedicated device memory).
 
────────────────────────────────────────────────────────────────────────────
 
21.8 Memory Interoperability Library aicore.ffi.mem ★ v0.2 Added
 
────────────────────────────────────────────────────────────────────────────
 
Security Type:
 
Buffer<T>                       A buffer with ownership (automatically dropped upon release)
 
Span<T> / Span<const T>         Borrowed views with length and access permissions (read-only/writable)
 
Span<mut T>                     Mutable borrow view
 
RawPtr<T>                       Restricted raw pointer (can only be derived from Buffer/Span)
 
MemoryView<T>                   External memory view (passed in from C/C++/Rust/Python)
 
SharedBuffer                    Inter-process / inter-language shared memory
 
Handle                          External memory/object handle (stored in the FFI handle table)
 
API Reference:
 
Category        Function / Method                                        Description
 
────────────────────────────────────────────────────────────────────
 
Allocation        mem::alloc(size, align, cap)                      Aligned allocation (requires a token)
 
mem::alloc_zeroed / alloc_pinned / alloc_device(dev)
 
mem::realloc / Buffer::free (or drop for automatic deallocation)
 
mem::arena(size) { ... } / mem::pool(capacity)     Arena/pool allocation
 
View        Buffer::as_span() / as_ptr() / as_mut_ptr() / into_raw()
 
Span::slice(range) / subspan() / cast::<U>() / split_at()
 
External access    mem::from_raw_parts(ptr, len)?                     Validate length and alignment before accessing
 
mem::from_cbuffer(ptr, len) / from_mmap(path)
 
mem::from_dlpack(capsule) / Tensor::to_dlpack()
 
SharedBuffer::create/open/map/unmap(name, size, cap)
 
Validation        mem::validate(ptr, len, align) / is_valid(span)
 
mem::check_device(ptr, Device) / guard_page()
 
Diagnosis        mem::stats() / mem::leaks() / mem::dump() / mem::poison(span)
 
mem::generation(ptr)                               Generation counting
 
────────────────────────────────────────────────────────────────────────────
 
21.9 Anti-Misuse Mechanism ★ v0.2 Added
 
────────────────────────────────────────────────────────────────────────────
 
Compile-time interception (zero cost):
 
• Switch not enabled → Compilation error (not a warning): A bare pointer at the off level triggers an error.
 
• Disallow implicit integer-to-pointer conversions: must use mem::unsafe_from_addr(addr, cap) at the full level.
 
• Typed pointers: RawPtr<T> and Span<T> carry the element type and length; a type mismatch results in a compile-time error.
 
• Permission annotation: Span<const T> is non-writable; assigning to read-only memory results in a compile-time error.
 
• Ownership/Borrowing: Buffer move semantics + borrowing checks to prevent dangling references; borrows must not outlive FFI calls.
 
• Alignment check: If alignment is not satisfied (at compile time or runtime), an error is reported immediately.
 
• Overflow checking: size * count is either evaluated at compile time via constant folding or checked for overflow at runtime.
 
• Device consistency: Passing a CPU pointer to the GPU API (or vice versa) → compile-time/runtime error.
 
Runtime protection (debug/sandboxed level):
 
• Boundary checks: Accessing an out-of-bounds Span index → panics (a safe failure, not undefined behavior).
 
• Double free: allocate an ID and set the freed flag; subsequent double frees → panic.
 
• Use-after-free: After-free tainting (0xDE padding) + generational counting to detect residual accesses.
 
• Canary/Sentinel: Sentinel words placed at the head and tail of the buffer to detect overflow writes.
 
• Leak detection: The process exited/the test ended, but the report did not release the buffer.
 
• Audit logs: Each raw memory operation is logged with who/what/size/addr/stack information; this feature can be enabled or disabled.
 
• Circuit Breaker: Continuous illegal access triggers the circuit breaker, automatically degrades to “off” and generates an alert.
 
• Isolation: The sandboxed level permits access only to registered areas; crossing these boundaries triggers a fault.
 
────────────────────────────────────────────────────────────────────────────
 
21.10 Memory Interoperability Example ★ v0.2 Added
 
────────────────────────────────────────────────────────────────────────────
 
Example 1: Enable the switch + capability token + pass to C
 
#![memory(level = "sandboxed")]                 # Explicitly enable sandbox-level memory interoperability
 
use aicore::ffi::mem 
fn share_with_c(x: Tensor<f32, [N]>) -> Result<(), MemError> { 
let cap = mem::capability(MemCap::Raw)?      # Obtain the capability token
 
let buf = mem::alloc(x.numel() * 4, align = 64, cap)?   # Aligned allocation
 
defer { buf.free() }                          # Ensure release (exception safety)
 
let span = buf.as_span::<f32>()               # View with type and length
 
span.copy_from(x.as_span())                   # Out-of-bounds or type mismatches will be intercepted
 
let rc = unsafe { c_consume(span.as_ptr(), span.len()) }  # Pass to the C function
 
if rc != 0 { return Err(MemError::from_c(rc)) }
 
Ok(())
 
}
 
Example 2: Safe Access to External Pointers (Read-Only Scope)
 
@memory_gate(level = "readonly") {                # Within the scope, only read access is allowed 
let view: Span<const f32> = mem::from_raw_parts(ptr, len)?  # Validate length/alignment
 
let s = view.sum()                            # Read-only; cannot be written to
 
}
 
Example 3: Misoperations are intercepted (at compile time / runtime)
 
# Assume the current setting is off (default)
 
mem::alloc(1024, cap)                 # ✗ Compilation error: memory allocation is not enabled
 
let p = 0x7fff_0000 as *f32           # ✗ Compilation error: implicit integer-to-pointer conversion is disallowed
 
let mut s: Span<const f32> = ...; s[0] = 1.0   # ✗ Compilation error: read-only view is not writable
 
mem::from_raw_parts(ptr, len)         # ✗ Compilation error: missing capability token
 
s[len]                                # ✗ Runtime panic: out of bounds (not UB)
 
buf.free(); buf.free()                # ✗ Runtime panic: double free
 
cuda_api(cpu_ptr)                     # ✗ Error: Passing a CPU pointer to a GPU API
 
Example 4: Cross-Language Shared Memory
 
let shm = SharedBuffer::create("aicore_job", size = 1 << 20, cap)?
 
shm.map()?                            # Maps to the current process (zero-copy)
 
# Python side: accessing shared memory or mmap with the same name
 
# C++ side: shm_open + mmap with the same name for access
 
# Rust side: memmap2 / shared_memory with the same name access
 
[22. Interoperability with Python]
 
22.1 Import and Invocation
 
@pyimport numpy as np           Import the Python module                        NumPy-compatible
 
@pyimport torch.nn as nn        PyTorch module                          Migrate existing code
 
@pyimport torch                 Import without alias                            torch.tensor(...)
 
from py.torch import nn         Equivalent syntax                              from syntax
 
py"..."                         Embed Python expressions                      Visualization/Plotting
 
py{ ... }                       Embed a Python code block                      Multi-line script
 
Example:
 
@pyimport numpy as np
 
@pyimport matplotlib.pyplot as plt 
let a = np.array([[1.0, 2.0], [3.0, 4.0]])   # Returns a PyObject
 
let t = torch.tensor([1.0, 2.0, 3.0])        # PyTorch tensor
 
# Inline expression (returns a Python object)
 
let s = py"sum([1, 2, 3])"                    # -> 6
 
# Multi-line code block (can capture AI-Core variables)
 
py{ 
import math
 
result = math.sqrt(2.0)
 
}
 
# Drawing 
plt.plot(np.arange(10), np.sin(np.arange(10)))
 
plt.savefig("plot.png")
 
22.2 Type Bridging and Zero-Copy
 
convert(x, PyObject)            Converts to a Python object                        Data exchange
 
convert(x, Tensor)              Converts back from a Python object                      Zero-copy
 
let np_arr = x.to_numpy()        # Tensor -> numpy.ndarray (DLPack zero-copy)
 
let t      = Tensor.from_numpy(np_arr)   # numpy -> Tensor (shared memory)
 
let torch_t = x.to_torch()       # Tensor <-> torch.Tensor (DLPack)
 
let back    = Tensor.from_torch(torch_t)
 
Py<T> typed wrapper:
 
let net: Py<nn.Module> = nn.Linear(10, 5)
 
let y = net(py_args(x))          # Call a Python callable object
 
22.3 GIL and Performance
 
with_gil { ... }                Acquire the GIL                             Call Python API
 
@py_gil fn f(x)                 The entire function holds the GIL                      Simple scenario
 
@py_no_gil fn heavy(x)          Release the GIL during the call (tensor computation)            Release parallelism
 
gil_release { tensor_op(x) }    Releases the GIL to perform re-computation                    Avoids blocking
 
Note: DLPack tensor operations (such as matmul) are executed by default with the GIL released.
 
Only multithreaded Python callers can achieve true parallelism.
 
22.4 Callbacks and Exporting to Python
 
@pyexport fn scale(x: Tensor, k: f32) -> Tensor   # Exposed for Python calls
 
return x * k
 
# Python side:
 
#   import aicore_ext 
#   y = aicore_ext.scale(arr, 2.0) 
@py_embed                       Embed the Python interpreter (run Python independently)
 
@py_init(venv = "./.venv")      Specifies the Python environment/virtual environment
 
@py_version ">=3.10"            Version constraint
 
22.5 Packaging as a Python Extension
 
[package]
 
name = "aicore_ext"
 
[lib]
 
crate-type = ["python-ext"]
 
aicore build --target python-ext   # Produces .so/.pyd (wheel)
 
aicore python install              # Install [python.dependencies]
 
22.6 Error Handling
 
let r: Result<Tensor, PyErr> = np.sqrt(x)
 
match r { 
Ok(v)  => use(v),
 
Err(e) => print(e.message()), 
}
 
[23. Interoperability with C/C++] (new in v0.2)
 
23.1 Declaring External C Functions
 
#[link(name = "mkl_rt", kind = "dylib")] 
#[include("mkl.h")]
 
extern "C" { 
fn cblas_sgemm(order: i32, transA: i32, transB: i32, 
M: i32, N: i32, K: i32, alpha: f32,
 
A: *const f32, lda: i32, 
B: *const f32, ldb: i32, beta: f32,
 
C: *mut f32, ldc: i32);
 
}
 
let mut C = zeros([M, N])
 
unsafe { cblas_sgemm(101, 111, 111, M, N, K, 1.0, A.ptr, K, 
B.ptr, N, 0.0, C.ptr_mut, N) }
 
23.2 Inline C++ Code
 
cxx"..."                        Inline C++ expression                       Returns a CxxObject
 
cxx{ ... }                      Inline C++ code block                       Multi-line
 
@cxx fn f(x) -> T               The entire function body is written in C++                  Performance hotspot
 
@cppclass Name { ... }          C++ class binding (RAII)                    Encapsulation
 
Example (expression):
 
let n = cxx"std::vector<int>{1,2,3}.size()"      # -> 3
 
Example (code block, accessible via Eigen):
 
@cxx fn fast_matmul(A: Tensor<f32,[M,K]>, B: Tensor<f32,[K,N]>) -> Tensor<f32,[M,N]>
 
{
 
# The following is C++ code
 
Eigen::Map<Eigen::MatrixXf> a(A.data(), M, K);
 
Eigen::Map<Eigen::MatrixXf> b(B.data(), K, N);
 
Eigen::MatrixXf c = a * b;
 
return Tensor<f32,[M,N]>::from_cpp(c.data(), M, N);
 
}
 
23.3 C++ Classes and Smart Pointers
 
@cppclass cv::Mat {
 
fn new(rows: i32, cols: i32, type: i32) -> cv::Mat   # Constructor
 
fn rows() -> i32
 
fn at(x: i32, y: i32) -> f64
 
# Destruction is handled automatically by RAII
 
}
 
let m = cv::Mat.new(480, 640, 16)
 
defer { m.free() }               # or rely on RAII
 
UniquePtr<T> / SharedPtr<T>     corresponds to std::unique_ptr / std::shared_ptr
 
WeakPtr<T>                      Corresponds to std::weak_ptr
 
23.4 Bind Generation (bindgen)
 
aicore cxx gen-bindings \ 
--header "opencv2/opencv.hpp" \ 
--include-dirs "/usr/include/opencv4" \ 
--out "src/opencv_bindings.aic"
 
# Automatic Generation of `extern "C"`/C++ Declarations, Struct Mappings, and RAII Wrapping
 
23.5 Layout and ABI Compatibility
 
#[repr(C)] struct Point { x: f32, y: f32 }     # Ensures C layout
 
#[packed] struct Header { tag: u8, len: u32 }  # Packed layout
 
#[repr(transparent)] struct Handle(usize)      # Single-field transparent
 
23.6 Building Integration
 
[cxx.dependencies] 
eigen  = { version = "3.4", system = true }
 
opencv = { version = "4.9", headers = "/usr/include/opencv4", 
libs = ["opencv_core", "opencv_imgproc"] }
 
myblas = { path = "./vendor/blas", build = "cmake" }
 
• When build = "cmake", the aicore build process invokes CMake to compile and link.
 
• Also supports build = "make" / "meson" / "bazel" / "custom" (specifying a script).
 
• Header file and library paths, linking flags, and compilation flags can be refined in [cxx].
 
23.7 Custom CUDA Operators (C++/CUDA)
 
@cxx fn fused_swiglu(x: Tensor<f16,[B,S,D]>) -> Tensor<f16,[B,S,D]> { 
// CUDA C++, including a __global__ kernel, processed at compile time by nvcc
 
}
 
[24. Interoperability with Rust] (added in v0.2)
 
24.1 Inline Rust Code
 
rust"..."                       Inline Rust expression                       Returns a Rust value
 
rust{ ... }                     Inline Rust code block                       Multi-line
 
@rust fn f(x) -> T              The entire function body is written in Rust                  Performance/Safety hotspot
 
Example:
 
let s: f32 = rust"
 
let v: Vec<f32> = vec![1.0, 2.0, 3.0];
 
v.iter().sum()
 
" 
 
@rust fn tokenize(text: &str) -> Vec<u32> { 
// You can directly use the crate from [rust.dependencies]
 
let enc = tokenizers::Tokenizer::from_file("tokenizer.json").unwrap();
 
enc.encode(text, true).unwrap().get_ids().to_vec()
 
}
 
24.2 Introducing a crate
 
@rustimport ndarray as nd       Import the Rust crate                        Alias
 
extern crate polars;            Equivalent syntax                              Use directly
 
use rust::rayon::prelude::*;    Use a module within the crate                       in parallel
 
[rust.dependencies] 
ndarray    = { crate = "ndarray", version = "0.15", features = ["rayon"] }
 
tokenizers = "0.19" 
candle     = { crate = "candle-core", version = "0.4" }
 
aicore rust vendor              Synchronize Cargo dependencies and generate Cargo.lock
 
aicore build will automatically invoke Cargo to compile the Rust portion (with incremental caching).
 
24.3 Types and Ownership Across Boundaries
 
AI-Core                 Rust
 
──────────────────────────────────────────────
 
&T (borrowing)               &T / &mut T
 
@move x                 T (move by value)
 
Box<T>                  Box<T>
 
Vec<T>                  Vec<T>
 
Arc<T> / Rc<T>          Arc<T> / Rc<T>
 
Option<T> / Result      Option<T> / Result
 
Tensor<T,S>             DLPack / Custom Tensor View
 
str                     &str / String
 
Rules:
 
• By default, borrowing is used and remains valid for the duration of the call; use @move or Arc if you need to hold it for a longer period.
 
• Box/Vec returned from the Rust side should be received using RustBox<T> / RustVec<T>, and can be converted with .into_vec().
 
• A cross‑boundary struct must be annotated with #[repr(C)].
 
• Unwinding across ABIs is prohibited at the boundary; panics are caught with catch_unwind and converted to Err.
 
let v: RustVec<f32> = rust"vec![1.0f32, 2.0, 3.0]"
 
let native: Vec<f32> = v.into_vec()      # Zero-copy transfer (if the allocator is compatible)
 
24.4 Exporting for Rust Calls
 
#[no_mangle] 
@rust_export fn aicore_forward(x: *const f32, len: usize, out: *mut f32) 
aicore build --crate-type rlib      # Produces a Rust crate (including .rs bindings)
 
# Rust side:
 
#   let lib = aicore_ext::load("model.aic")?; 
#   let y = lib.forward(&x)?;
 
24.5 unsafe boundary
 
unsafe { ... }                  Unsafe block (pointer dereferencing, calling extern functions)      Explicitly marked
 
@safe_wrapper                   Generates safe wrappers for unsafe FFI                Recommended
 
[25. Interoperability with Julia]
 
AI-Core can invoke Julia code:
 
@juliacall using SpecialFunctions  # Import the Julia package
 
@juliacall besselj(2, x)           # Call a Julia function
 
Julia can call libraries compiled by AI-Core:
 
using AICore.jl  # Julia wrapper
 
model = load("model.aic")  # Load the AI-Core model
 
Extending Julia Interoperability (v0.2):
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
@jlimport SpecialFunctions      Import the Julia module                       Alias support
 
jl"sum(1:10)"                   Inline Julia expression                      Returns a JlObject
 
jl{ ... }                       Inline Julia code block                      Multi-line
 
@jl fn solve(A, b) -> Vector    The function body is written in Julia                     Multiple dispatch interoperability
 
@juliacall expr                 Evaluate a Julia expression or function                  Lightweight bridging
 
Type mapping:
 
AI-Core          Julia
 
──────────────────────────────────────────────
 
Tensor<T,S>      Array{T,N} / CuArray (DLPack zero-copy)
 
f32 / f64        Float32 / Float64
 
i32 / i64        Int32 / Int64
 
Vec<T>           Vector{T}
 
HashMap          Dict
 
Option           Union{T,Nothing} 
Result           Union{T, Exception} 
Dependency and Embedding:
 
[julia.dependencies] 
SpecialFunctions      = "2.4" 
DifferentialEquations = "7.10"
 
aicore julia instantiate        # Synchronize Julia dependencies (Project/Manifest.toml)
 
@jl_embed                       # Embed libjulia (run Julia within an independent process)
 
@jl_threads 8                   # Specify the number of Julia threads (inter-language parallelism)
 
Multiple Dispatch Coordination: Both AI-Core and Julia are multiple-dispatch languages, enabling bidirectional registration at the boundaries.
 
Methods (registering AI-Core functions as Julia methods, or vice versa).
 
[26. Interoperability with Other Ecosystems] (Added in v0.2)
 
Ecology            Grammar/Mechanism                          Description
 
──────────────────────────────────────────────────────────────
 
Java/JVM        @jvmimport / jni"..."              Called via JNI; @jvm_export for export
 
java "System.currentTimeMillis()" is supported on Android.
 
JavaScript/WASM wasm"..." / @wasm_export          Compiles to WASM; in the wasm-bindgen style
 
js "performance.now()" Browser/Node call
 
Fortran         @fortran module / fortran"..."     iso_c_binding; scientific computing library
 
Go              @goimport / go"..."                 cgo; call a Go service/library
 
R               @rimport / r"..."                   Rcpp style; statistical ecology
 
Swift/Kotlin    @swift_export / @kotlin_export      Mobile native library (C ABI)
 
Zig             @zig / zig"..."                     C ABI compatible, compile-time metaprogramming
 
MATLAB/Octave   @matlab / matlab"..."               .mex bridge
 
General-purpose exchange formats: ONNX / StableHLO / DLPack / Arrow — for cross-framework tensor and model interchange
 
SafeTensors / GGUF                  Weight format
 
Note: All ownership/error/type mapping conventions for the entire ecosystem share [21]; only Python/C++/
 
Rust and Julia provide “inline source code” capabilities, while the rest primarily rely on the C ABI or standard exchange formats.
 
════════════════════════════════════════════════════════════════════════════
 
Part Five: Custom Code and Extensions
 
════════════════════════════════════════════════════════════════════════════
 
[27. Custom Kernels and Inline Assembly] (added in v0.2, expanded from [9])
 
27.1 GPU Kernel DSL
 
@kernel fn matmul_tile(A, B, C) 
row = @block_idx(0) * @block_dim(0) + @thread_id(0)
 
col = @block_idx(1) * @block_dim(1) + @thread_id(1)
 
tile_a = @shared_mem [16, 16]f32
 
tile_b = @shared_mem [16, 16]f32
 
acc = 0.0f32
 
for k in 0..(@K / 16) {
 
tile_a[@thread_id(0), @thread_id(1)] = A[row, k*16 + @thread_id(1)]
 
tile_b[@thread_id(0), @thread_id(1)] = B[k*16 + @thread_id(0), col]
 
@sync_threads()
 
for i in 0..16 {
 
acc += tile_a[@thread_id(0), i] * tile_b[i, @thread_id(1)]
 
}
 
@sync_threads()
 
}
 
C[row, col] = acc
 
Kernel primitives:
 
@thread_id(d) / @block_idx(d) / @block_dim(d) / @grid_dim(d) / @warp_id()
 
@lane_id() / @sync_threads() / @shared_mem / @atomic_add / @atomic_cas
 
@warp_shuffle(x, lane) / @ballot(pred) / @cooperative_groups()
 
27.2 Inline Assembly
 
asm"mov eax, 1"                 Inline assembly expression                         Extreme optimization
 
asm{ ... }                      Inline assembly block                             Multiple instructions
 
@asm fn f(x)                    Function body written in assembly                            Manually optimized hotspots
 
asm volatile / asm inline       Modifier                                 Semantic Control
 
Example:
 
@asm fn popcount(x: u64) -> u64 {
 
asm "popcnt rax, rdi"        # Return in rax
 
}
 
27.3 Inline C++/CUDA/Rust Kernels (Cross-Language Hotspots)
 
@cxx fn warp_reduce(x: Tensor<f32,[N]>) -> Tensor<f32,[1]> { /* CUDA C++ */ }
 
@rust fn simd_dot(a: &[f32], b: &[f32]) -> f32 { /* Rust + std::simd */ }
 
[28. Custom Operators and Automatic Differentiation Extensions] (New in v0.2)
 
28.1 Custom Operators
 
@op silu(x: Tensor) -> Tensor
 
forward:  x * sigmoid(x)
 
backward: g -> g * (sigmoid(x) * (1 + x * (1 - sigmoid(x))))
 
@op my_attention(Q, K, V) -> Tensor
 
forward:  softmax(Q @ K.T / sqrt(D)) @ V
 
backward: @autograd_derive            # Automatic backward derivation
 
@register_op "my::fused_ops"              # Register with the operator library (can be implemented across languages) 
@cpu_impl / @cuda_impl / @tpu_impl        # Specify the backend implementation
 
28.2 Custom Automatic Differentiation Functions
 
@grad_fn
 
struct MyFunc {
 
fn forward(ctx: GradContext, x: Tensor) -> Tensor {
 
ctx.save_for_backward(x)
 
return x * x + 1
 
}
 
fn backward(ctx: GradContext, g: Tensor) -> Tensor {
 
let x = ctx.saved()[0]
 
return g * 2 * x
 
}
 
}
 
@custom_vjp fn f(x) -> (Tensor)          # Manual VJP (reverse-mode rule)
 
primal: x * x
 
vjp:    (x, g) -> g * 2 * x
 
@custom_jvp fn f(x) -> (Tensor)          # Manual JVP (forward rule) 
jvp: (x, v) -> 2 * x * v
 
@nondiff fn argmax(x) -> Tensor          # Mark as non-differentiable
 
@stop_gradient / detach                   # Truncate gradients
 
28.3 Custom Layers/Modules
 
struct MyBlock <: Module {
 
weight: Parameter<Tensor<f32,[I,O]>>
 
fn forward(self, x) -> Tensor {
 
return silu(x @ self.weight)
 
}
 
}
 
@register_module MyBlock
 
[29. Custom Compiler Passes and the Backend] (Added in v0.2)
 
29.1 Custom IR Pass
 
@pass struct MyFusionPass {
 
fn run(module: MLIRModule) -> MLIRModule {
 
for op in module.ops() {
 
if op.is("linalg.matmul") && op.next().is("linalg.generic") { 
op.fuse_with(op.next())
 
}
 
}
 
return module
 
}
 
}
 
@register_pass MyFusionPass stage="post-optimize" 
Pass stages: pre-lower / shape-infer / autodiff / optimize / lower / codegen
 
Built-in Passes: constant folding, CSE, operator fusion, DCE, memory layout, loop optimizations,
 
Vectorization, parallelization, and layout optimization
 
29.2 Customizing MLIR Dialects
 
@dialect "ai_core.custom" { 
on "custom.silu"(x: tensor) -> tensor
 
on "custom.attention"(q: tensor, k: tensor, v: tensor) -> tensor
 
lower_to "linalg" { ... }        # Downgrade to the standard dialect
 
}
 
29.3 Customizing the Compilation Backend
 
impl Backend for MyBackend {
 
fn name() -> str { "my_backend" }
 
fn supports(device: Device) -> bool { device == Device::NPU(0) }
 
fn compile(graph: Graph, opts: CompileOpts) -> CompiledKernel { ... }
 
fn execute(kernel: CompiledKernel, inputs: Vec<Tensor>) -> Vec<Tensor> { ... }
 
}
 
@register_backend MyBackend
 
@compile target=MyBackend        # Use a custom backend
 
29.4 Build Scripts and Code Generation
 
build.aic                       Build script (executed before compilation)                  Generate code / Validate
 
@codegen fn gen_kernels()       Code generation function                            Templated kernel
 
@include! / @embed!             Compile-time inclusion/embedding of files                      Resource packaging
 
Example build.aic:
 
fn main() {
 
let spec = read_json("ops.json")? 
for op in spec.ops {
 
write_file(f"src/generated/{op.name}.aic", gen_op(op))
 
}
 
}
 
[30. Export AI-Core as a library in other languages] (added in v0.2)
 
AI-Core’s compiled artifacts can be used as libraries and invoked by other languages.
 
Goal            Reaction                                    Product
 
──────────────────────────────────────────────────────────────
 
C ABI shared library    aicore build --crate-type cdylib         libmodel.so/.dll/.dylib + model.h
 
Static library          aicore build --crate-type staticlib      libmodel.a
 
Python extension     aicore build --target python-ext         model-0.1-cp311-*.whl（.so/.pyd）
 
Rust crate      aicore build --crate-type rlib           libmodel.rlib + model.rs
 
Julia package        aicore build --target julia-pkg          Model.jl + Manifest.toml
 
WASM            aicore build --target wasm               model.wasm + model.d.ts
 
ONNX            aicore export --format onnx              model.onnx
 
StableHLO       aicore export --format stablehlo         model.mlir
 
Mobile: aicore build --target android/ios .aar / .xcframework
 
Service            aicore build --target server             REST/gRPC service image
 
The export interface is declared using an annotation:
 
@export_c_api                    # Generate C header files and a stable ABI
 
@export_python name="model"      # Generate Python bindings 
@export_rust                     # Generate Rust bindings 
@export_julia                    # Generate Julia bindings
 
@abi_version "1.0"               # ABI version (ensures compatibility)
 
Example of the generated C header file (model.h):
 
typedef struct ModelHandle ModelHandle;                 // Opaque handle 
ModelHandle* model_load(const char* path, const char* err, size_t err_len);
 
int model_forward(ModelHandle* h, const float* x, size_t n, float* out); 
void model_free(ModelHandle* h);
 
Safety Contract: All exported functions must not panic across the ABI; errors are returned via return codes and error buffers.
 
The caller is responsible for releasing the handle (or providing a *_free function).
 
════════════════════════════════════════════════════════════════════════════
 
Part Six: Appendices
 
════════════════════════════════════════════════════════════════════════════
 
[31. Comparison of Compiler Architectures and Compilation Models]
 
Compilation Pipeline (v0.2):
 
Source code .aic
 
│
 
▼ Lexer            Lexical analysis (including indentation/macros)
 
▼ Parser           Recursive Descent + Macro Expansion → AST
 
▼ Resolver         Module resolution / Name resolution / Package dependencies
 
▼ TypeChecker Type and shape checking (enforced at compile time)
 
▼ Autodiff         Automatic differentiation graph construction (vjp/jvp/custom)
 
▼  HIR → MIR        High-level/Middle-level IR (SSA)
 
▼ Optimizer        Constant folding/CSE/operator fusion/DCE/memory layout/loop optimization
 
▼ MLIR             Dialect lowering (linalg/arith/...)
 
▼ Backends         LLVM / CUDA / XLA / ROCm / Metal / NPU / WASM
 
▼  Products             Executable / Library / ONNX / Python Extension / Service
 
Feature                 AI-Core                 Julia                   Python
 
─────────────────────────────────────────────────────────────────────────────
 
Execution Model                AOT/JIT Compilation             JIT Compilation                 Interpretation Execution
 
Compilation Target                MLIR/LLVM/CUDA/XLA      LLVM                    - (Interpreted)
 
Type Checking                Compile-time + Run-time           Run-time (optional compile-time)      Run-time
 
Automatic Differentiation    Native    Via Zygote.jl    Via PyTorch/TF
 
Multiple dispatch                Native (core feature)         Native (core feature)          Single dispatch
 
Metaprogramming                  Native Macros                  Native Macros                  Decorators/Meta-classes
 
GPU support                 Native kernel                CUDA.jl                 CuPy/Numba
 
Distributed                  Native syntax                Via Distributed.jl        Via Horovod
 
Deployment                    Native export to ONNX/TensorRT   via PackageCompiler       TorchScript/ONNX
 
Module/Package System             Built-in (Cargo style)        Built-in (Pkg)             pip/venv
 
Cross-language interoperability            Native four-language inline FFI        ccall/PyCall/...         ctypes/cffi
 
[32. Unique Features of AI-Core]
 
Grammar Point                          Function                                Example
 
──────────────────────────────────────────────
 
Tensor<T, Shape>                Shape as a type parameter                      Compile-time checking
 
x |> f |> g                     Pipeline operator                           Chained operations
 
@shape [B, C, H, W]             Shape annotation macro                           Documentation/Validation
 
@compile target=GPU             Compilation target annotation                          Hardware specification
 
@autodiff mode=reverse          Automatic differentiation mode                          Forward/backward
 
@quantize bits=8                Quantization annotation                             Automatic INT8
 
@prune sparsity=0.5             Pruning annotation                             Sparsification
 
@distributed                    Distributed annotation                           Parallelism strategy
 
@checkpoint                     Checkpoint annotation                           Memory optimization
 
v0.2 New exclusive features:
 
• Native inline support for four languages: cxx"..." / rust"..." / jl"..." / py"..." along with @cxx/@rust/@jl functions.
 
• Multilingual dependency management: An AI-Core.toml file manages Python, C++, Rust, and Julia dependencies all at once.
 
• Compile-time shape checking + cross-language tensor zero-copy (DLPack) interoperability.
 
• First-class extension APIs for custom operators, automatic differentiation, compiler passes, and backends.
 
• One-click multi-target export (C ABI / Python / Rust / Julia / WASM / ONNX).
 
[33. Type Inference and Errors]
 
The AI-Core compiler performs checks at compile time:
 
• Tensor shape matching (matrix multiplication dimensions)
 
• Device consistency (CPU and GPU tensors cannot be mixed)
 
• Type stability (avoiding the `Any` type)
 
• Differentiability (ensuring differentiability)
 
• Module visibility (private item out-of-bounds access)
 
• FFI type mapping reversibility (cross-language types must be mappable)
 
• Ownership/Legality of Borrowing (Cross-Boundary Lifecycle)
 
Incorrect example:
 
let x: Tensor<f32, [3, 4]> = ...
 
let y: Tensor<f32, [5, 6]> = ...
 
let z = x @ y  # Compilation error: inner product dimensions do not match (4 vs 5)
 
Correct way:
 
let z = x @ y.transpose()  # OK: [3,4] @ [6,5] -> Needs adjustment
 
let z = x @ y.T            # OK: [3,4] @ [4,6] -> [3,6]
 
Example of an interoperability error:
 
@pyimport numpy as np
 
let v = np.array([1,2,3])
 
let t: Tensor<f32,[3]> = convert(v, Tensor)   # OK: int64 -> requires explicit cast
 
# If the shapes don't match: a "FFI shape mismatch: expected [3], got [4]" error is reported at compile time/runtime
 
[34. Performance Optimization Tips]
 
AI-Core provides a variety of compiler hints:
 
@inbounds x[i]              # Disable bounds checking (ensure no out-of-bounds access) 
@simd for i in 0..N         # Force SIMD vectorization
 
@unroll for i in 0..4       # Loop unrolling (small loop)
 
@inline fn f(x)             # Force inlining
 
@noinline fn f(x)           # Disable inlining (for debugging/ recursion)
 
@fastmath x * y             # Fast math (trade precision for speed)
 
@no_grad fn inference(x)    # Disable gradient computation
 
@prefetch / @aligned        # Prefetch/Alignment
 
@cuda_graph                 # Capture the compute graph
 
@amp dtype=bf16             # Mixed precision
 
Interoperability Performance Tips:
 
• Tensors should prefer DLPack zero-copy (to avoid the copy path of `to_numpy`/`to_torch`).
 
• Batch invocation instead of element-by-element cross‑boundary calls (reducing FFI overhead).
 
• Release the GIL during heavy computation (@py_no_gil / gil_release).
 
• Inline hot functions in C++/Rust to avoid boundary round-trips.
 
• Use AICore Profile to identify cross‑category spending.
 
[35. Key Differences from Julia]
 
Feature                    AI-Core                         Julia
 
───────────────────────────────────────────── 
Main Areas                AI/ML-Specific                        General Scientific Computing
 
Tensor type                A first-class citizen built into the language                  Implemented via Array
 
Automatic differentiation Native syntax (backward) Via the Zygote.jl macro
 
Device Management                Native Device Type                    Wrapped via CUDA.jl
 
Model Definition                Specialized model DSL                     Ordinary struct + functions
 
Training workflow                Dedicated train DSL                     Standard Julia code
 
Deployment Export                Native Multi-Format Export                    Via PackageCompiler
 
Shape checking                Enforced at compile time (core feature)             Optional (via macros)
 
Distributed                  Native annotation syntax                      Via the Distributed module
 
Kernel Programming                @kernel DSL                       CUDA.jl/KernelAbstractions
 
Package Management                  Built-in + Multilingual Dependencies                  Pkg (Julia Ecosystem)
 
Cross-language                 Four-language native inline                     ccall/PyCall (requires adaptation)
 
[36. Complete Example (Including Interoperability)]
 
Example A: ResNet-50 (Training + Export)
 
model ResNet50 { 
conv1: Conv2d(3, 64, kernel=7, stride=2, padding=3) 
bn1: BatchNorm(64) 
relu: ReLU()
 
maxpool: MaxPool2d(kernel=3, stride=2, padding=1)
 
layer1: ResidualLayer(64, 64, 3)
 
layer2: ResidualLayer(64, 128, 4, stride=2)
 
layer3: ResidualLayer(128, 256, 6, stride=2)
 
layer4: ResidualLayer(256, 512, 3, stride=2)
 
avgpool: AdaptiveAvgPool2d([1, 1])
 
fc: Linear(512, 1000)
 
fn forward(x: Tensor<f32, [B, 3, 224, 224]>)
 
-> Tensor<f32, [B, 1000]> { 
x = x |> self.conv1 |> self.bn1 |> self.relu
 
|> self.maxpool
 
x = self.layer1(x)
 
x = self.layer2(x)
 
x = self.layer3(x)
 
x = self.layer4(x)
 
x = x |> self.avgpool |> flatten |> self.fc
 
return x
 
}
 
}
 
fn train() {
 
let model = ResNet50().to(Device::CUDA(0))
 
let optimizer = Adam(model.parameters(), lr=0.001)
 
let data = ImageNet.load(batch_size=256)
 
for epoch in 0..90 {
 
for (images, labels) in data {
 
@with_grad { 
let logits = model(images)
 
let loss = cross_entropy(logits, labels) 
}
 
backward(loss)
 
optimizer.step()
 
optimizer.zero_grad()
 
}
 
}
 
# Export Deployment
 
model.eval()
 
serialize(model, "resnet50.aic")
 
export_onnx(model, "resnet50.onnx")
 
}
 
Example B: Four-Language Collaboration (Python Data + C++ Hotspots + Rust Tools + Julia Numerics)
 
# ── Python: Data Loading and Visualization ───────────────────────────────
 
@pyimport numpy as np
 
@pyimport torch
 
@pyimport matplotlib.pyplot as plt 
# ── C++: Hot Operators (Eigen Acceleration)─────────────────────────────
 
@cxx fn fast_matmul(A: Tensor<f32,[M,K]>, B: Tensor<f32,[K,N]>) -> Tensor<f32,[M,N]> { 
Eigen::Map<Eigen::MatrixXf> a(A.data(), M, K);
 
Eigen::Map<Eigen::MatrixXf> b(B.data(), K, N);
 
Eigen::MatrixXf c = a * b;
 
return Tensor<f32,[M,N]>::from_cpp(c.data(), M, N);
 
}
 
# ── Rust: Tokenization and Parallel Processing ───────────────────────────────────
 
@rust fn tokenize(text: &str) -> Vec<u32> { 
let enc = tokenizers::Tokenizer::from_file("tokenizer.json").unwrap();
 
enc.encode(text, true).unwrap().get_ids().to_vec()
 
}
 
# ── Julia: Special Functions and Differential Equations ──────────────────────────────
 
@juliacall using SpecialFunctions
 
@juliacall using DifferentialEquations
 
fn main() {
 
# Data on the Python side -> AI-Core tensor (DLPack zero-copy)
 
let raw = np.random.randn(128, 784).astype("float32")
 
let x: Tensor<f32,[128,784]> = Tensor.from_numpy(raw)
 
# C++ Hotspot Calculation
 
let w = randn([784, 256])
 
let h = fast_matmul(x, w)
 
# Rust Tokenization
 
let ids = tokenize("hello ai-core world")
 
# Julia Special Functions
 
let b = @juliacall besselj(2, 3.0)
 
# Return Python Plot (Zero-Copy)
 
plt.plot(h.sum(axis=1).to_numpy())
 
plt.savefig("out.png")
 
}
 
Example C: Custom Operator + Export to a C Library
 
@op silu(x: Tensor) -> Tensor
 
forward:  x * sigmoid(x)
 
backward: g -> g * (sigmoid(x) * (1 + x * (1 - sigmoid(x))))
 
model Tiny { 
fc: Linear(784, 10)
 
fn forward(x) -> Tensor { return silu(self.fc(x)) }
 
}
 
@export_c_api
 
@export_python name="tiny" 
fn predict(x: Tensor<f32,[B,784]>) -> Tensor<f32,<B,10]> {
 
return Tiny().load("tiny.aic").forward(x)
 
}
 
# Build: aicore build --crate-type cdylib --target python-ext
 
# Output: libtiny.so + tiny.h + tiny-0.1-cp311-*.whl
 
[37. Grammar Quick Reference] (Added in v0.2)
 
Category            Keyword / Symbol
 
──────────────────────────────────────────────────────────────
 
Declaration            let / var / val / const / fn / struct / model / layer /
 
trait / impl / type / mod / use / pub / opaque / newtype
 
Type            Tensor / Scalar / Vector / Matrix / Graph / Device /
 
Option / Result / Union / Vec / HashMap / Box / Arc / Handle
 
Control flow          if / else / match / for / while / loop / break / continue /
 
return / defer / try / catch / finally
 
Arithmetic            + - * / % ** @ |> && || ! == != < <= > >= .. ..= ?. ?
 
Ownership          & / &mut / @move / @borrow / @copy / @view / 'a / 'static
 
Metaprogramming          macro / quote / $ / @generated / @derive / @include! / @lang
 
Automatic differentiation        grad / backward / jacobian / hessian / vjp / jvp /
 
@grad / @custom_vjp / @custom_jvp / @no_grad / @checkpoint
 
Distributed          @distributed / all_reduce / all_gather / reduce_scatter /
 
scatter / gather / barrier / launch
 
Hardware            @kernel / @parallel / @simd / @unroll / @thread_id /
 
@shared_mem / @sync_threads / @atomic_add / @cuda_graph
 
Modules and Packages        mod / use / pub(crate) / prelude / #[cfg] / AI-Core.toml / aicore
 
Python interoperability   @pyimport / py"..." / py{ } / @pyexport / @py_gil / @py_embed /
 
Py<T> / PyObject / convert / to_numpy / from_numpy
 
C/C++ Interoperability    extern "C" / #[link] / #[include] / cxx"..." / cxx{ } / @cxx /
 
@cppclass / UniquePtr / SharedPtr / #[repr(C)] / unsafe
 
Rust interoperability     @rustimport / rust"..." / rust{ } / @rust / RustBox / RustVec /
 
extern crate / #[no_mangle]
 
Julia interoperability    @juliacall / @jlimport / jl"..." / jl{ } / @jl / @jl_embed
 
Other interoperability      @jvmimport / jni"..." / wasm"..." / @wasm_export /
 
@fortran / @goimport / @rimport / @zig / @matlab
 
Memory interoperability      #![memory(level=off|readonly|sandboxed|full)] / #[memory] /
 
@memory_gate / [memory] Inventory / mem::capability / Buffer / Span /
 
RawPtr / from_raw_parts / from_dlpack / SharedBuffer / mem::validate
 
Custom extensions      @op / @register_op / @grad_fn / @pass / @dialect /
 
@register_backend / @plugin / asm"..." / @export_c_api
 
Toolchain          aicore new/build/run/test/bench/fmt/lint/doc/repl/doctor/
 
python install/cxx gen-bindings/export/publish
 
[Summary]
 
AI-Core = Julia’s multiple dispatch + Python’s simplicity + C++’s performance + Rust’s safety + AI-native design
 
Goal: To become the “Julia” of AI development—while maintaining the flexibility of a general-purpose language,
 
It also achieves framework-level ease of use through domain-specific optimizations, such as domain-specific languages, automatic differentiation, and hardware abstraction.
 
The core proposition of v0.2:
 
• Language as a Platform: A built-in module system, package management, and multi-language dependencies enable full-stack development with a single manifest. 
• Interoperability as a first-class citizen: source-level inlining and zero-copy tensors for Python, C, C++, Rust, and Julia.
 
• Scalable architecture: Custom operators, automatic differentiation rules, compiler passes, and backends are all open-source.
 
• Write once, deploy across multiple platforms: One-click export to C ABI, Python, Rust, Julia, WASM, and ONNX.
 
══════════════════════════════════════════════════════════════
 
[38. Grammar Extension v0.3]
 
══════════════════════════════════════════════════════════════
 
The syntax of this section is fully supported by `aicorec` (a Rust implementation): from source code → C++17 → native executable.
 
A comprehensive example can be found in `aic compiler/example/syntax2.aic`.
 
38.1 enum Enumeration
 
──────────────────────────────────────────────────────────────
 
Grammar:
 
enum name { variant1, variant2, ... }
 
enum name { variant1 = 1, variant2 }      # Integer values can be explicitly specified
 
Enumeration value. Variant                          # Example: Color::Green
 
Example:
 
enum Color { Red, Green, Blue }
 
let c = Color::Green
 
if c == Color::Green { print("green") }
 
print(c)                             # Output the variant name Green
 
Description: Generates a C++ `enum class`; supports assignment, argument passing, comparison, pattern matching, and printing.
 
38.2 impl block
 
──────────────────────────────────────────────────────────────
 
Grammar:
 
impl type name {
 
fn method_name(self, parameters...) -> return_type { ... }
 
}
 
Example:
 
struct Point { x: f64
 
y: f64 }
 
impl Point {
 
fn len2(self) -> f64 { 
return self.x * self.x + self.y * self.y
 
}
 
}
 
print(Point(3.0, 4.0).len2())        # 25 
Note: Methods defined within an `impl` block are entirely equivalent to those defined directly inside the struct; `self` serves as the receiver and is not treated as a C++ parameter.
 
It also supports adding methods to the model within an impl block.
 
38.3 Struct Construction and Field Read/Write
 
──────────────────────────────────────────────────────────────
 
Grammar:
 
Type name(value1, value2, ...)                # In the order of field declaration
 
Type name { field: value, ... }             # Named fields (you can specify only some fields; the rest will be set to their default values)
 
obj.field                             # Read 
obj.field = new_value                       # Write
 
Example:
 
struct Point { x: f64
 
y: f64 }
 
let p = Point(3.0, 4.0)
 
var q = Point { x: 1.0, y: 2.0 }
 
q.x = 5.0
 
print(q.x)                           # 5
 
print(p.len2())                      # 25 
Field types are mapped according to annotations: f32/f64 -> double, i32/i64 -> long.
 
str/String -> std::string, bool -> bool, Tensor<...> -> Tensor.
 
38.4 else if
 
──────────────────────────────────────────────────────────────
 
Grammar:
 
if condition { ... } elif condition { ... } else { ... }
 
Example:
 
if score >= 90 { print("A") }
 
elif score >= 80 { print("B") }
 
else { print("C") }
 
Indentation style can also be used:
 
if score >= 90
 
print("A")
 
elif score >= 80
 
print("B")
 
else
 
print("C")
 
38.5 assert Assertion
 
──────────────────────────────────────────────────────────────
 
Grammar:
 
assert(condition)
 
assert(condition, "failure message")
 
Example:
 
assert(score > 0, "score must be positive")
 
Note: On failure, a C++ exception of type std::runtime_error is thrown, which is caught and printed by the main function.
 
`error: <message>` is sent to stderr, and the process exits with a non-zero status.
 
38.6 String Methods
 
──────────────────────────────────────────────────────────────
 
Grammar (all can be called in a chained manner):
 
s.len()  s.upper()  s.lower()  s.trim()
 
s.contains(sub)  s.starts_with(p)  s.ends_with(p)
 
s.replace(a, b)  s.substr(i, n)
 
s.split(sep)                         # Returns a list of strings
 
s.parse_int()  s.parse_float()       # Throws an error on parsing failure
 
Global functions:
 
str(x)                               # Any value -> string
 
len(x)                               # Length of a string / list / tensor
 
Example:
 
let s = "  AI-Core  "
 
print(s.trim().upper())              # AI-CORE 
print(s.contains("Core"))            # true
 
print("a,b,c".split(",").len())      # 3
 
print("42".parse_int() + 1)          # 43
 
for part in "a,b,c".split(",") { print(part) }
 
38.7 Mathematical Built-ins (Scalar)
 
──────────────────────────────────────────────────────────────
 
sqrt abs pow exp ln log10
 
sin cos tan asin acos atan
 
floor ceil round
 
min max clamp sign hypot
 
Example:
 
print(sqrt(16.0))                    # 4
 
print(abs(-3))                       # 3
 
print(pow(2.0, 10.0))                # 1024 
print(clamp(5.0, 0.0, 3.0))          # 3
 
print(hypot(3.0, 4.0))               # 5
 
Note: Functions with the same name retain tensor overloading (e.g., sqrt, abs, sign, and clamp can also operate on Tensors).
 
When used with a single argument, min/max reduces a list; when used with two arguments, it returns the scalar minimum or maximum.
 
38.8 List
 
──────────────────────────────────────────────────────────────
 
Grammar:
 
[1, 2, 3]                            # List literal
 
xs.len() / len(xs)                   # length
 
xs[i]                                # Read (starting from 0)
 
for x in xs { ... }                  # Iterate
 
sum(xs) / mean(xs) / min(xs) / max(xs)
 
Example:
 
let xs = [1.0, 2.0, 3.0, 4.0]
 
print(xs.len())                      # 4
 
print(xs[0] + xs[3])                 # 5
 
var total = 0.0
 
for x in xs { total += x }
 
print(total)                         # 10
 
print(sum(xs))                       # 10
 
Note: In the generated code, the list of numerical values is reused as a Tensor representation (one-dimensional), and therefore can be used with
 
Tensor operations, reshape, softmax, and other built-in functions are interoperable.
 
38.9 for range
 
──────────────────────────────────────────────────────────────
 
for i in 0..n { ... }                # [0, n)
 
for i in 0..=n { ... }               # [0, n]
 
Example:
 
var acc = 0
 
for i in 0..4 { acc += i }           # 0+1+2+3 = 6
 
var acc2 = 0
 
for i in 0..=4 { acc2 += i }         # 0+1+2+3+4 = 10
 
38.10 Closures and map / filter / fold
 
──────────────────────────────────────────────────────────────
 
Grammar:
 
|Parameter| Expression
 
|Parameter: Type| -> Return Type { Statement... }
 
map(xs, |x| x * 2)
 
filter(xs, |x| x > 0)
 
fold(xs, initial value, |a, b| a + b)
 
Example:
 
let f = |x| x * 2
 
print(f(21))                         # 42
 
let g = |x: f64| -> f64 { x + 1 }
 
print(g(41.0))                       # 42 
let xs = [1.0, 2.0, 3.0, 4.0]
 
print(map(xs, |x| x * 2))            # Tensor[4] = [2, 4, 6, 8] 
print(filter(xs, |x| x > 2))         # Tensor[2] = [3, 4]
 
print(fold(xs, 0.0, |a, b| a + b))   # 10
 
Note: When no type annotation is provided, a C++ generic lambda `[=](auto x){ ... }` is generated;
 
When type annotations are present, concrete type arguments and a return type are generated. Closures can be passed as values and invoked.
 
38.11 match guard
 
──────────────────────────────────────────────────────────────
 
Grammar:
 
match x { 
Pattern if condition => result,
 
}
 
Example:
 
let word = match n { 
0 => "zero",
 
n if n > 0 => "positive", 
_ => "negative"
 
}
 
38.12 String Concatenation
 
──────────────────────────────────────────────────────────────
 
"a" + "b"                            # String concatenation
 
"n = " + str(42)                     # Non-string automatically converted to string
 
Example:
 
let greeting = "Hello" + ", " + "AI-Core"
 
print(greeting)                      # Hello, AI-Core
 
print("n = " + str(42))              # n = 42
 
Note: As long as either side of the `+` operator is syntactically a string literal, an f-string, or the result of a string method,
 
It is handled as string concatenation (internally calling to_str); when both sides are variables, the native C++ `+` operator is preserved.
 
Semantics (two std::strings can still be concatenated normally).
 
38.13 Known Limitations
 
──────────────────────────────────────────────────────────────
 
• Enum variants currently do not support carrying payloads (you cannot write `Option::Some(x)`).
 
• String list literals (e.g., `["a", "b"]`) are not yet supported; the return value of `split()` can be directly iterated over or indexed.
 
• Closures use C++’s `[=]` capture (capture by value); explicit capture lists and capture of mutable references are not yet supported.
 
• Type checking remains lenient: unknown types do not trigger errors; no deep type inference is performed except for matmul and shape declarations.
 
• The numeric list reuses a Tensor (stored as float), so the precision of the list elements is f32.
 
