// TASK 2.1: GetProductById - COMPLETAMENTO
[HttpGet("{id}")]
public async Task<ActionResult<Product>> GetProductById(int id)
{
    var product = await _context.Products
        .AsNoTracking()
        .Include(p => p.ProductTags)
            .ThenInclude(pt => pt.Tag)
        .Include(p => p.Category)
        .SingleOrDefaultAsync(p => p.ProductId == id);
    
    if (product == null)
    {
        return NotFound();
    }
    
    return Ok(product);
}

// TASK 2.2: SearchProducts (10 Points)
[HttpGet("search")]
public async Task<ActionResult<IEnumerable<Product>>> SearchProducts(
    [FromQuery] string? searchTerm,
    [FromQuery] decimal? minPrice)
{
    // 1. Start with IQueryable
    var query = _context.Products.AsQueryable();
    
    // 2. If searchTerm is provided, filter by Name OR Description
    if (!string.IsNullOrWhiteSpace(searchTerm))
    {
        query = query.Where(p => p.Name.Contains(searchTerm) || 
                                 p.Description.Contains(searchTerm));
    }
    
    // 3. If minPrice is provided, filter by Price
    if (minPrice.HasValue)
    {
        query = query.Where(p => p.Price >= minPrice.Value);
    }
    
    // 4. Use AsNoTracking for optimization
    query = query.AsNoTracking();
    
    // 5. Execute the query with ToListAsync and return results
    var products = await query.ToListAsync();
    
    return Ok(products);
}

// TASK 2.3: GetCategoryStatistics (10 Points)
[HttpGet("statistics")]
public async Task<ActionResult<IActionResult>> GetCategoryStatistics()
{
    // 1. Query the Categories
    var statistics = await _context.Categories
        .AsNoTracking()
        // 2. Use .Select() to project results into anonymous object (or DTO)
        .Select(category => new
        {
            // 3. The object should have:
            CategoryName = category.Name,  // From category.Name
            ProductCount = category.Products.Count()  // Using category.Products.Count()
        })
        // 4. Execute and return the list
        .ToListAsync();
    
    return Ok(statistics);
}