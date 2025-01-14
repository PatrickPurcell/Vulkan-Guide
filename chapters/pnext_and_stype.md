# pNext and sType

People new to Vulkan will start to notice the `pNext` and `sType` variables all around the Vulkan Spec.

The `void* pNext` is used to allow for expanding the Vulkan Spec by creating a Linked List between structures. The [pNext valid usage](https://www.khronos.org/registry/vulkan/specs/1.1/html/vkspec.html#fundamentals-validusage-pNext) section of the Vulkan Spec goes into details explaining the two different variations of `pNext` structures. The `VkStructureType sType` is used by the loader, layers, and implementations to know what type of struct was passed in by `pNext`. The use of `pNext` is mostly used when dealing with extensions that expose new structures.

An example to help illustrate the use of `pNext`

```
// An example with two structures, "a" and "b"
// These structs have members you'd find in real Vulkan structures
typedef struct VkA {
    VkStructureType sType;
    void* pNext;
    uint32_t value;
} VkA;

typedef struct VkB {
    VkStructureType sType;
    void* pNext;
    uint32_t value;
} VkB;

// A Vulkan Function that takes struct "a" as an argument
// This function is in charge of populating the values
void vkGetValue(VkA* pA);

// Define "a" and "b" and set their sType
struct VkB b = {};
b.sType = VK_STRUCTURE_TYPE_B;

struct VkA a = {};
a.sType = VK_STRUCTURE_TYPE_A;

// Set the pNext pointer from "a" to "b"
a.pNext = (void*)&b;

// Pass "a" to the function
vkGetValue(&a);

// Use the values which were both set from vkGetValue()
printf("VkA value = %u \n", a.value);
printf("VkB value = %u \n", b.value);
```
