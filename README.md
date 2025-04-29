# A Merkle / Hash Tree Experiment with the Ruby Programming Language

A very simple implementation of a Merkle Tree in Ruby, focusing on the basic ideas.

## First Try
Each data item is hashed (SHA256).
Leaves are paired, concatenated, and then hashed again to form parent nodes.
If there is an odd number of nodes, the last one is duplicated to make a pair.
This continues until a single root hash remains: the Merkle Root.

```ruby
require 'digest'

class MerkleTree
  attr_reader :root

  def initialize(data_list)
    leaves = data_list.map { |data| Digest::SHA256.hexdigest(data) }
    @root = build_tree(leaves)
  end

  private

  def build_tree(nodes)
    return nodes.first if nodes.size == 1

    # If odd number of nodes, duplicate last node
    nodes << nodes.last if nodes.size.odd?

    parent_nodes = []

    nodes.each_slice(2) do |left, right|
      combined = left + right
      parent_nodes << Digest::SHA256.hexdigest(combined)
    end

    build_tree(parent_nodes)
  end
end

# Example usage
data = ["a", "b", "c", "d"]
tree = MerkleTree.new(data)
puts "Merkle Root: #{tree.root}"

```
