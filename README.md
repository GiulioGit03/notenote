UsingEntity(
        j => j
            .HasOne(typeof(Course))
            .WithMany()
            .HasForeignKey("CourseId"),
        j => j
            .HasOne(typeof(Student))
            .WithMany()
            .HasForeignKey("StudentId"),
        j =>
        {
            j.HasKey("StudentId", "CourseId");
            j.ToTable("StudentCourses");
        }